# volatile과 synchronized

## volatile 키워드

- `volatile` 키워드는 `멀티쓰레드 환경`에서 변수의 `가시성`을 보장하는 키워드
- CPU Cache가 아닌 `Main Memory`에 직접 접근하여 값을 읽고 쓰기 때문에 `쓰레드 간의 가시성`을 보장
- 하나의 쓰레드가 값을 변경하면 다른 쓰레드가 즉시 변경된 값을 읽을 수 있음
- 여러 쓰레드가 값을 변경할 때, `synchronized`로 원자성을 보장해줘야 함

## synchronized

- 멀티 스레드 환경에서는 여러 스레드가 공유자원에 동시에 접근할 수 있음
- `경쟁 상태`(race condition)이 발생할 수 있음
- synchronized는 임계영역(Critical Section)에 대해 Lock을 걸어 공유자원에 대한 `동시접근을 막는 동기화`

## synchronized 내부

### 모니터 락(Monitor Lock)

- synchronized는 `모니터 락(Monitor Lock)`을 사용
- 모니터 락은 `객체 단위`로 존재
  - Java 객체들은 `모니터`라는 메타데이터를 하나씩 가짐
- 모니터 락은 `오직 하나의 스레드만 소유`할 수 있음
- 모니터 락을 소유한 스레드가 모니터 락을 해제하기 전까지 다른 스레드는 `대기`

### 내부 동작

- 자바 바이트코드 레벨에서 synchronized 키워드는 `monitorenter`와 `monitorexit` 명령어로 구현
- monitorenter
  - 스레드가 객체의 모니터를 `획득`할 때 호출
  - 만약 모니터가 다른 스레드에 의해 이미 잠겨 있다면, 해당 스레드는 모니터를 획득할 때까지 대기
- monitorexit
  - 스레드가 모니터를 `해제`할 때 호출
  - 일반적으로 synchronized 블록 또는 메서드의 끝에서 실행

## Lock의 범위

### 메서드에 synchronized 키워드 추가

```java
public class Test {
	public synchronized void test() {
		// 생략
	}
}
```

- 메서드가 포함된 객체(this)에 Lock이 걸림
- Object 레벨의 Lock
- 만약 `static 메서드`에 synchronized 키워드를 추가하면 Class 레벨의 Lock


### 블록에 synchronized 키워드 추가

```java
public class Test {
	public void test() {
		synchronized (Product.class) {
            // 생략
        }
		
		// 생략
	}
}
```

- 괄호안에 인자로 넣는 객체가 Lock이 걸림
- Class 레벨의 Lock
- 만약 인스턴스 레벨의 Lock을 걸고 싶다면 `this`를 넣으면 됨

## wait(), notify(), notifyAll()

- Object에 정의되어 있는 메서드
- synchronized 블록 내에서만 사용 가능
- 효율적인 동기화 지원
- `wait()`: waiting pool로 이동
- `notify()`: waiting pool에서 임의의 스레드 하나만 깨움

## synchronized로 동시성 제어하기

- synchronized를 사용하면 `동시성 문제`를 해결할 수 있음
- 하지만 `성능 저하`가 발생할 수 있음
  - `Race Condition`
    - notify()는 하나를 임의로 선택해서 통지할 뿐이고, 여러 쓰레드가 lock를 얻기 위해 경쟁
  - `Starvation`
    - notify()는 무작위로 선택되기 때문에, 특정 쓰레드가 계속해서 선택되지 않을 수 있음
    - notifyAll()을 사용해도 결국 Lock은 하나의 쓰레드만 획득 가능
- 낮은 지연시간이 요구되거나 분산 환경에서는 다른 방법을 고려하자.
  - `ReentrantLock`과 `StampedLock`을 사용하면 성능을 향상시킬 수 있음
  - 분산락 활용
