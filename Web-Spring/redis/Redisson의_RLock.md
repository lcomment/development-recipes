# Redisson의 RLock

Redisson을 활용하여 동시성 이슈를 해결할 때 기본적으로 다음과 같이 코드를 작성할 수 있다.

```java
public Object lock(
	String key,
	long waitTime,
	long leaseTime,
	TimeUnit timeUnit
) {
	final RLock lock = redissonClient.getLock(lockName);

	try {
		boolean available = rLock.tryLock(waitTime, leaseTime, timeUnit);  // (2)

		if (!available) {
			return false;
		}

		// 비즈니스 로직 수행

	} catch (InterruptedException e) {
		throw new InterruptedException();
	} finally {
		try {
			rLock.unlock();
		} catch (IllegalMonitorStateException e) {
			log.info("Redisson Lock Already UnLock: {}", key);
		}
	}
}
```

## Redisson의 기본적인 RLock 동작방식

### RedissonClient의 getLock()

```java
public interface RedissonClient {
	// 생략

	RLock getLock(String name);

	// 생략
}
```

RedissonClient의 getLock() 메서드는 RLock 인터페이스를 리턴하고 있고, RedissonClient의 구현체는 다음과 같다.

```java
public class Redisson implements RedissonClient {

	protected final CommandAsyncExecutor commandExecutor;

	// 생략

	@Override
	public RLock getLock(String name) {
		return new RedissonLock(commandExecutor, name);
	}

	// 생략
}
```

### RedissonLock

RedissonLock은 RedissonBaseLock 추상 클래스를 구현하고 있는데, RedissonBaseLock은 다음과 같다.

```java
public abstract class RedissonBaseLock extends RedissonExpirable implements RLock {
	// 생략
}
```

따라서, RedissonLock은 RLock 인터페이스를 구현하고 있다. RLock 인터페이스는 tryLock() 메서드를 가지고 있다.

```java
public interface RLock extends Lock, RLockAsync {
	// 생략

	boolean tryLock(long waitTime, long leaseTime, TimeUnit unit) throws InterruptedException;

	// 생략
}
```

- `waitTime`: 락 획득을 시도하는 최대 대기 시간
- `leaseTime`: 락을 획득한 이후 자동으로 해제되는 시간
- `unit`: 위 두 시간의 단위 (초, 밀리초, 분 등)

RedissonLock은 tryLock()을 구현하고 있다.

```java
public class RedissonLock extends RedissonBaseLock {
	// 생략

	@Override
	public boolean tryLock(long waitTime, long leaseTime, TimeUnit unit) throws InterruptedException {
		long time = unit.toMillis(waitTime);
		long current = System.currentTimeMillis();
		long threadId = Thread.currentThread().getId();
		Long ttl = tryAcquire(waitTime, leaseTime, unit, threadId);

		if (ttl == null) {
			return true;
		}

		time -= System.currentTimeMillis() - current;
		if (time <= 0) {
			acquireFailed(waitTime, unit, threadId);
			return false;
		}

		current = System.currentTimeMillis();
		CompletableFuture<RedissonLockEntry> subscribeFuture = subscribe(threadId);
		try {
			subscribeFuture.get(time, TimeUnit.MILLISECONDS);
		} catch (TimeoutException e) {
			if (!subscribeFuture.cancel(false)) {
				subscribeFuture.whenComplete((res, ex) -> {
					if (ex == null) {
						unsubscribe(res, threadId);
					}
				});
			}
			acquireFailed(waitTime, unit, threadId);
			return false;
		} catch (ExecutionException e) {
			acquireFailed(waitTime, unit, threadId);
			return false;
		}

		try {
			time -= System.currentTimeMillis() - current;
			if (time <= 0) {
				acquireFailed(waitTime, unit, threadId);
				return false;
			}

			while (true) {
				long currentTime = System.currentTimeMillis();
				ttl = tryAcquire(waitTime, leaseTime, unit, threadId);

				if (ttl == null) {
					return true;
				}

				time -= System.currentTimeMillis() - currentTime;
				if (time <= 0) {
					acquireFailed(waitTime, unit, threadId);
					return false;
				}

				currentTime = System.currentTimeMillis();
				if (ttl >= 0 && ttl < time) {
					commandExecutor.getNow(subscribeFuture).getLatch().tryAcquire(ttl, TimeUnit.MILLISECONDS);
				} else {
					commandExecutor.getNow(subscribeFuture).getLatch().tryAcquire(time, TimeUnit.MILLISECONDS);
				}

				time -= System.currentTimeMillis() - currentTime;
				if (time <= 0) {
					acquireFailed(waitTime, unit, threadId);
					return false;
				}
			}
		} finally {
			unsubscribe(commandExecutor.getNow(subscribeFuture), threadId);
		}
	}

	// 생략
}
```

- 락이 비어 있으면 바로 획득 (true)
- 아니면 waitTime 동안 락이 풀릴 때까지 기다림 (pub/sub)
- 락이 풀리면 다시 시도
- 시간이 초과되면 false 반환

```java
public class RedissonLock extends RedissonBaseLock {
	// 생략

	private Long tryAcquire(long waitTime, long leaseTime, TimeUnit unit, long threadId) {
		return get(tryAcquireAsync(waitTime, leaseTime, unit, threadId));
	}

	private <T> RFuture<Long> tryAcquireAsync(long waitTime, long leaseTime, TimeUnit unit, long threadId) {
		RFuture<Long> ttlRemainingFuture;
		if (leaseTime > 0) {
			ttlRemainingFuture = tryLockInnerAsync(waitTime, leaseTime, unit, threadId, RedisCommands.EVAL_LONG);
		} else {
			ttlRemainingFuture = tryLockInnerAsync(waitTime, internalLockLeaseTime,
				TimeUnit.MILLISECONDS, threadId, RedisCommands.EVAL_LONG);
		}
		CompletionStage<Long> f = ttlRemainingFuture.thenApply(ttlRemaining -> {
			// lock acquired
			if (ttlRemaining == null) {
				if (leaseTime > 0) {
					internalLockLeaseTime = unit.toMillis(leaseTime);
				} else {
					scheduleExpirationRenewal(threadId);
				}
			}
			return ttlRemaining;
		});
		return new CompletableFutureWrapper<>(f);
	}
}
```

1. tryAcquire() → 락 시도를 위해 tryAcquireAsync() 호출
2. tryAcquireAsync()에서 leaseTime 조건 따라 tryLockInnerAsync() 호출
3. Redis Lua 스크립트로 락 획득 시도
4. 락 성공 시:
    - leaseTime이 명시되면 설정 저장
    - leaseTime이 0이면 watchdog 예약
5. 결과값(ttl or null)을 동기 방식으로 리턴 (RFuture.get())

```java
public class RedissonLock extends RedissonBaseLock {
	// 생략

	<T> RFuture<T> tryLockInnerAsync(long waitTime, long leaseTime, TimeUnit unit, long threadId, RedisStrictCommand<T> command) {
		return evalWriteAsync(getRawName(), LongCodec.INSTANCE, command,
			"if (redis.call('exists', KEYS[1]) == 0) then " +
			"redis.call('hincrby', KEYS[1], ARGV[2], 1); " +
			"redis.call('pexpire', KEYS[1], ARGV[1]); " +
			"return nil; " +
			"end; " +
			"if (redis.call('hexists', KEYS[1], ARGV[2]) == 1) then " +
			"redis.call('hincrby', KEYS[1], ARGV[2], 1); " +
			"redis.call('pexpire', KEYS[1], ARGV[1]); " +
			"return nil; " +
			"end; " +
			"return redis.call('pttl', KEYS[1]);",
			Collections.singletonList(getRawName()), unit.toMillis(leaseTime), getLockName(threadId));
	}

	// 생략
}
```

1. 락이 아예 없음 → 락 생성 + TTL 설정 → null 반환 (성공)
2. 내가 이미 락 보유 → 재진입 허용, 횟수 + TTL 갱신 → null 반환 (성공)
3. 다른 스레드가 보유 → TTL 확인 → TTL(ms) 반환 (실패)

## RLock의 구현체로 RedissonLock을 사용할 때 장점

- 사용 간편성과 안정성 (기본이자 표준)
  - RedissonClient.getLock("key")으로 바로 사용 가능 
  - 대부분의 실무 환경에서 복잡한 조건 없이 충분히 안정적
- `재진입 가능` (Reentrant)
  - 한 스레드가 여러 번 락을 획득해도 누적 횟수 관리됨 
  - unlock() 시 횟수가 0이 될 때만 실제 락 해제됨
- `자동 만료` + Watchdog 지원
  - leaseTime == 0이면 내부적으로 watchdog이 자동 갱신 
    - 기본 갱신 주기: 10초 
    - 기본 TTL: 30초 → 작업이 끝나기 전 락이 풀리는 문제 방지
- `성능` 우수 (멀티 Redis보다 빠름)
  - 단일 Redis에만 의존하므로 RedissonMultiLock보다 네트워크 비용이 낮음 
  - 락 획득과 TTL 갱신이 빠르고 간단한 연산
- `pub/sub` 기반 wait queue 지원
   - tryLock()이 내부적으로 Redis pub/sub을 활용한 알림 대기 방식으로 효율적인 락 대기 가능 
   - 폴링 방식보다 훨씬 자원 효율적
