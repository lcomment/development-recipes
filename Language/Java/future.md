# Java의 Future

### Future란?

- JDK 1.5에서 나온 인터페이스
- `비동기`적 연산 작업을 위해 만들어진 인터페이스
- `멀티 스레드` 환경에서 처리된 어떤 데이터를 다른 스레드에 전달할 수 있음
- 내부적으로 `Thread-Safe` 하게 구현되어 있음 → synchronized block을 사용하지 않아도 됨

```java
public interface Future<V> {

	/*
	 * 작업을 취소하려고 시도함
	 * @param mayInterruptIfRunning 작업이 실행 중일 때 취소할지 여부
	 */
	boolean cancel(boolean mayInterruptIfRunning);

	/*
	 * 작업이 취소되었는지 여부를 반환
	 * @return 작업이 취소되었으면 true, 아니면 false
	 */
	boolean isCancelled();

	/*
	 * 작업이 완료되었는지 여부를 반환
	 * @return 작업이 완료되었으면 true, 아니면 false
	 */
	boolean isDone();

	/*
	 * 작업의 결과를 반환
	 * @return 작업의 결과
	 */
	V get() throws InterruptedException, ExecutionException;

	/*
	 * 작업의 결과를 반환
	 * @param timeout 작업이 완료될 때까지 기다리는 시간
	 * @param unit timeout의 단위
	 * @return 작업의 결과
	 */
	V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;
}
```

### 장점

- 비동기 작업의 결과를 쉽게 관리 가능
- 기다리지 않고, 결과가 필요할 때 가져올 수 있음
- `isDone()` 메서드를 통해 작업이 완료됐는지 확인 가능

### 단점

- 작업 완료 알림의 부재
  - polling 방식이라 계속 메서드를 호출해 작업 완료 여부를 확인해야 함
- blocking 방식
  - `get()` 메서드를 호출하면 작업이 완료될 때까지 블로킹됨

### 결론: JDK 8의 [CompletableFuture](https://github.com/lcomment/development-recipes/blob/main/Language/Java/completableFuture.md)을 사용하자.
