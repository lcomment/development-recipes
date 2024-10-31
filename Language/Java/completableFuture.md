# CompletableFuture

### Future의 한계

- get의 타임아웃 설정으로만 완료 가능
- 블로킹 코드(get)를 통해서만 이후의 결과를 처리할 수 있음
- 여러 연산을 조합할 수 없음
- 비동기 처리 중 발생하는 예외에 대한 처리가 힘듦

### CompletableFuture

- Future의 한계를 극복하기 위해 Java 8에서 등장
- 기존의 Future를 기반으로 `외부에서 완료`를 수행할 수 있도록 설계
- 콜백 등록 및 여러 연산 조합 가능

| 비교       | Future   | CompletableFuture |
|----------|----------|-------------------|
| 방식       | blocking | non-blocking      |
| 연산 조합    | 불가능      | 가능                |
| 연산 결과 결합 | 불가능      | 가능                |
| 예외 처리    | 불가능      | 가능                |

## CompletableFuture 인스턴스 생성

```java
/*
 * Runnable을 비동기로 실행하고 완료된 CompletableFuture를 반환
 * @param runnable 비동기로 실행할 Runnable
 */
public static CompletableFuture<Void> runAsync(Runnable runnable) {
	return asyncRunStage(ASYNC_POOL, runnable);
}

/*
 * Supplier를 비동기로 실행하고 완료된 CompletableFuture를 반환
 * @param supplier 비동기로 실행할 Supplier
 */
public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier) {
	return asyncSupplyStage(ASYNC_POOL, supplier);
}
```

### 예제

```java
@Test
void runAsync() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<Void> future = CompletableFuture.runAsync(
        () -> userClient.save(mobileNumber)
    );

    // then
    assertDoesNotThrow(() -> future.get());
}

@Test
void supplyAsync() {
	// given
	final String mobileNumber = "010-1234-5678";
	
	// when
    CompletableFuture<Void> future = CompletableFuture.runAsync(
		() -> userClient.find(mobileNumber)
    );

    // then
    assertEquals(expectedUser, future.get());
}
```

## 연산 순차 처리

```java
/*
 * 반환 값을 받아서 다른 값을 반환함
 */
public <U> CompletableFuture<U> thenApply(Function<? super T,? extends U> fn) {
    return uniApplyStage(null, fn);
}

/*
 * 반환 값을 받아 처리하고 값을 반환하지 않음
 */
public CompletableFuture<Void> thenAccept(Consumer<? super T> action) {
    return uniAcceptStage(null, action);
}

/*
 * 반환 값을 받지 않고 다른 작업을 실행함
 */
public CompletableFuture<Void> thenRun(Runnable action) {
    return uniRunStage(null, action);
}
```

### 예제

```java
@Test
void thenApply() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<User> future = CompletableFuture.supplyAsync(
        () -> userClient.find(mobileNumber)
    ).thenApply(
        user -> userClient.update(user)
    );

    // then
    assertEquals(expectedUser, future.get());
}

    
@Test
void thenAccept() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<Void> future = CompletableFuture.supplyAsync(
        () -> userClient.find(mobileNumber)
    ).thenAccept(
        user -> userClient.delete(user)
    );

    // then
    assertDoesNotThrow(() -> future.get());
}

@Test
void thenRun() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<Void> future = CompletableFuture.supplyAsync(
        () -> userClient.find(mobileNumber)
    ).thenRun(
        () -> userClient.writeFindLog(mobileNumber)
    );

    // then
    assertDoesNotThrow(() -> future.get());
}
```

## 연산 조합

```java
/*
 * 두 CompletableFuture을 순차적으로 결합하여 앞선 작업의 결과를 받아서 사용
 */
public <U> CompletableFuture<U> thenCompose(Function<? super T, ? extends CompletionStage<U>> fn) {
    return uniComposeStage(null, fn);
}

/*
 * 두 CompletableFuture을 결합하여 두 작업의 결과를 받아서 사용
 */
public <U,V> CompletableFuture<V> thenCombine(CompletionStage<? extends U> other, BiFunction<? super T,? super U,? extends V> fn) {
	return biApplyStage(null, other, fn);
}

/*
 * 두 CompletableFuture을 결합하여 두 작업의 결과를 받지 않고 다른 작업을 실행
 */
public <U> CompletableFuture<Void> thenAcceptBoth(CompletionStage<? extends U> other, BiConsumer<? super T, ? super U> action) {
	return biAcceptStage(null, other, action);
}
```

### 예제

```java
@Test
void thenCompose() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<User> future = CompletableFuture.supplyAsync(
        () -> userClient.find(mobileNumber)
    ).thenCompose(
        user -> userClient.updateAsync(user)
    );

    // then
    assertEquals(expectedUser, future.get());
}

@Test
void thenCombine() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<User> future = CompletableFuture.supplyAsync(
        () -> userClient.find(mobileNumber)
    ).thenCombine(
        CompletableFuture.supplyAsync(() -> userClient.find(mobileNumber)),
        (user1, user2) -> userClient.merge(user1, user2)
    );

    // then
    assertEquals(expectedUser, future.get());
}

@Test
void thenAcceptBoth() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<Void> future = CompletableFuture.supplyAsync(
        () -> userClient.find(mobileNumber)
    ).thenAcceptBoth(
        CompletableFuture.supplyAsync(() -> userClient.find(mobileNumber)),
        (user1, user2) -> userClient.merge(user1, user2)
    );

    // then
    assertDoesNotThrow(() -> future.get());
}
```

## 병렬 처리

```java
public static CompletableFuture<Void> allOf(CompletableFuture<?>... cfs) {
        return andTree(cfs, 0, cfs.length - 1);
}

public static CompletableFuture<Object> anyOf(CompletableFuture<?>... cfs) {
	int n; Object r;
	if ((n = cfs.length) <= 1)
		return (n == 0)
			? new CompletableFuture<Object>()
			: uniCopyStage(cfs[0]);
	for (CompletableFuture<?> cf : cfs)
		if ((r = cf.result) != null)
			return new CompletableFuture<Object>(encodeRelay(r));
	cfs = cfs.clone();
	CompletableFuture<Object> d = new CompletableFuture<>();
	for (CompletableFuture<?> cf : cfs)
		cf.unipush(new AnyOf(d, cf, cfs));
	// If d was completed while we were adding completions, we should
	// clean the stack of any sources that may have had completions
	// pushed on their stack after d was completed.
	if (d.result != null)
		for (int i = 0, len = cfs.length; i < len; i++)
			if (cfs[i].result != null)
				for (i++; i < len; i++)
					if (cfs[i].result == null)
						cfs[i].cleanStack();
	return d;
}
```

### 예제

```java
@Test
void allOf() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<Void> future = CompletableFuture.allOf(
        CompletableFuture.runAsync(() -> userClient.find(mobileNumber)),
        CompletableFuture.runAsync(() -> userClient.find(mobileNumber))
    );

    // then
    assertDoesNotThrow(() -> future.get());
}

@Test
void anyOf() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<Object> future = CompletableFuture.anyOf(
        CompletableFuture.runAsync(() -> userClient.find(mobileNumber)),
        CompletableFuture.runAsync(() -> userClient.find(mobileNumber))
    );

    // then
    assertDoesNotThrow(() -> future.get());
}
```

## 예외 처리

```java
/*
 * 결과를 반환받아 에러가 발생한 경우와 아닌 경우 모두를 처리
 */
public <U> CompletableFuture<U> handle(BiFunction<? super T, Throwable, ? extends U> fn) {
	return uniHandleStage(null, fn);
}

/*
 * 발생한 에러를 받아서 예외 처리
 */
public CompletableFuture<T> exceptionally(Function<Throwable, ? extends T> fn) {
    return uniExceptionallyStage(null, fn);
}

/*
 * 아직 완료되지 않았으면 get을 바로 호출하고, 실패 시에 주어진 예외 발생
 */
public boolean completeExceptionally(Throwable ex) {
        if (ex == null) throw new NullPointerException();
        boolean triggered = internalComplete(new AltResult(ex));
        postComplete();
        return triggered;
}
```

### 예제

```java
@Test
void handle() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<User> future = CompletableFuture.supplyAsync(
        () -> userClient.find(mobileNumber)
    ).handle(
        (user, ex) -> {
            if (ex != null) {
                log.error("error", ex);
                return null;
            }
            return userClient.update(user);
        }
    );

    // then
    assertEquals(expectedUser, future.get());
}

@Test
void exceptionally() {
    // given
    final String mobileNumber = "010-1234-5678";
    
    // when
    CompletableFuture<User> future = CompletableFuture.supplyAsync(
        () -> userClient.find(mobileNumber)
    ).exceptionally(
        ex -> {
            log.error("error", ex);
            return null;
        }
    );

    // then
    assertEquals(expectedUser, future.get());
}
```
