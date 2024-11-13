# Executor와 ExecutorService

## Executor 인터페이스

- `쓰레드 풀의 구현`을 위한 인터페이스
  - 쓰레드 풀을 사용하면 쓰레드 생성 및 종료에 따른 오버헤드를 줄일 수 있음
- 쓰레드 작업 등록과 실행 중 실행에 대한 책임을 가짐 (ISP 준수)

```java
@Test
void executorRun() {
    final Runnable runnable = () -> System.out.println(Thread.currentThread().getName());
    final Executor executor = new TestExecutor();
	
    executor.execute(runnable);
}

static class TestExecutor implements Executor {

    @Override
    public void execute(final Runnable r) {
        new Thread(r).start();
    }
}
```

## ExecutorService 인터페이스

```java
public interface ExecutorService extends Executor, AutoCloseable {
	// 생략
}
```

- `작업(Runnable, Callable) 등록`을 위한 인터페이스
- Executor 인터페이스를 상속 받기에 `작업 실행에 대한 책임`도 가짐
- 작업 종료 후 다음 작업을 하기 위해 대기하기 때문에 `shutdown()` 메서드를 통해 명시적으로 종료시켜야 함

```java
@Test
void test() {
    ExecutorService executorService = Executors.newFixedThreadPool(32);
    Runnable runnable = () -> System.out.println(Thread.currentThread().getName());
	
    executorService.execute(runnable);
    executorService.shutdown();
}
```
