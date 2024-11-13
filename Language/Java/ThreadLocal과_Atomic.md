# ThreadLocal과 Atomic

&nbsp; 멀티스레드 프로그래밍에서는 전역 변수 사용을 지양한다. 전역 변수 사용 시, 동기화 문제, 데드 등의 문제가 발생하기 때문이다. 이러한 문제 때문에 Java에서는 대안을 제공하고 있고, 그 중 하나가 바로 `ThreadLocal`과 `Atomic 변수` 이다.

## ThreadLocal

- Java에서 멀티스레드 프로그래밍을 할 때 사용되는 클래스
- 오직 한 쓰레드에 의해 읽고 쓰여지는 변
- 여러 쓰레드가 같은 코드에서 실행되고, 같은 ThreadLocal을 참조해도 서로의 ThreadLocal을 볼 수도, 영향을 줄 수도 없음

### 활용 예시

- Spring Security에서 각 사용자의 인증 정보 사용

```java
final class ThreadLocalSecurityContextHolderStrategy implements SecurityContextHolderStrategy {
	private static final ThreadLocal<Supplier<SecurityContext>> contextHolder = new ThreadLocal();

	// 생략
}
```

### 주의사항

- 메모리 누수의 원인이 될 수 있으므로, 사용 후 반드시 삭제하자.

## Atomic

- `ThreadLocal`과 달리 `Atomic`은 `원자성`을 보장하는 클래스
- Non-blocking`으로 동작
- 쓰레드들이 suspend 되지 않기 때문에 `Context Switching이 발생하지 않음

### CAS 알고리즘 (Compare And Swap)

- 변수의 값을 변경하기 전에 `기존 값`이 내가 예상하던 값과 같을 경우에만 `새로운 값으로 할당`하는 방법
- `Atomic` 클래스는 `CAS` 알고리즘을 사용

### AtomicInteger의 내부 코드

```java
public class AtomicInteger extends Number implements java.io.Serializable {
	private static final long serialVersionUID = 6214790243416807050L;
	private static final Unsafe U = Unsafe.getUnsafe();
	private static final long VALUE = U.objectFieldOffset(AtomicInteger.class, "value");

	private volatile int value;
	
	// 생략

	public final int incrementAndGet() {
		return U.getAndAddInt(this, VALUE, 1) + 1;
	}
	
	// 생략

	public final int get() {
		return value;
	}

	public final void set(int newValue) {
		value = newValue;
	}
	
	// 생략
}

public final class Unsafe {
	// 생략
    
	@IntrinsicCandidate
	public final int getAndAddInt(Object o, long offset, int delta) {
		int v;
		do {
			v = getIntVolatile(o, offset);
		} while (!weakCompareAndSetInt(o, offset, v, v + delta));
		return v;
	}
	
	// 생략
}
```

- value 변수를 `volatile`로 선언
- UnSafe 클래스를 통해 `CAS` 알고리즘을 사용
- `getAndAddInt` 메서드를 통해 `AtomicInteger`의 값을 변경
- get()과 set() 자체가 `Atomic 연산`이라 Race Condition이 발생하지 않음
