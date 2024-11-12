# 쓰레드

## Thread 클래스를 상속 받는 방법

### Thread 클래스 내부 코드

```java
public class Thread implements Runnable {
    
    // 생략

    Thread(ThreadGroup g, String name, int characteristics, Runnable task, long stackSize, AccessControlContext acc) {
        // 생략
    }

    public Thread(Runnable task) {
        this(null, null, 0, task, 0, null);
    }
    
    // 생략
}
```

### Thread 구현하기

```java
class ThreadEx extends Thread {
    
    public void run() {
        System.out.println("Thread Ex");
    }
}

class Test {

    public static void main(String[] args) {
        THreadEx t = new ThreadEx();
        
        t.start();
    }
}
```

## Runnable 인터페이스를 구현하는 방법

- 보통 Runnable을 구현하여 쓰레드 구현
  - Thread 클래스를 상속 받으면 다른 클래스를 상속 받을 수 없음
  - 인터페이스 구현이 재사용성이 높고 코드 일관성 유지가 좋음

```java
class ThreadEx implements Runnable {
    
    public void run() {
        System.out.println("Thread Ex");
    }
}

class Test {

    public static void main(String[] args) {
        Runnable r = new ThreadEx();
        Thread t = new Thread(r);
        
        t.start();
    }
}
```

## start()와 run()

### run()을 실행한 경우

- 생성된 쓰레드를 실행시키는게 아님
- 단순히 클래스에 선언된 메서드를 호출한 것
- Call Stack에 main 위에 run 메서드가 쌓임
  - main에서 run을 호출

### start()를 실행한 경우

- main에 start가 쌓임
- 실행된 쓰레드의 호출 스택이 생성됨
- 쓰레드의 호출 스택에 run이 쌓임
  - 쓰레드가 run 호출
- run 수행 종료 후 쓰레드의 호출 스택도 사라짐
