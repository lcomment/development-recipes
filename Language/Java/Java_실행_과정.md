# Java의 실행과정

## JDK, JRE, JVM

### JDK

- 자바 개발 환경 (가장 큰 범위)
- 자바 컴파일러(javac), 자바 클래스 파일을 해석하는 javap

### JRE

- 자바 실행 환경
- JVM, 자바 클래스 라이브러리, 기타 실행에 필요한 파일 포함

### JVM

- 자바 애플리케이션을 실행하는 가상 머신
- 컴퓨터 자원(메모리)을 할당 받아 `Runtime Data Area` 구성
- `인터프리터`와 `JIT 컴파일러`를 통해 바이트 코드를 각 OS에 맞는 기계어로 해석
  - 인터프리터
    - 바이트코드 명령어를 하나씩 읽어서 실행
  - JIT(Just in time)
    - 바이트코드 전체를 컴파일해 네이티브 코드로 변경
    - 이후 해당 메서드를 더 이상 인터프리팅하지 않고 네이티브 코드로 직접 실행하는 방식
- `GC`를 통해 동적 메모리 관리

## Java 실행과정

<div align=center>
    <img src='../../resources/java/executeJava.png'>
    <h5>출처: https://steady-snail.tistory.com/67</h5>
</div>

<br>

> ### 1. `.java` → `.class`

- `javac`라는 Java Compiler에 의해 바이트 코드로 변환
  - `Byte Code`: 사람이 작성한 `소스코드`와 기계어의 중간 단계
- 어떤 프로세서에서도 작동할 수 있도록 기계어가 아닌 바이트 코드로 변환함

<br>

> ### 2. 컴파일 된 바이트 코드를 JVM의 `Class Loader`로 전달

- 동적 로딩을 통해 필요한 클래스들을 로딩 및 링크
- JVM의 메모리인 `Runtime Data Area`로 런타임 데이터를 올림

<br>

> ### 3. Execute

- `Execution Engine`은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 가져와 실행

<br>

### Reference

- [@whitepro](https://whitepro.tistory.com/458)
- [@steady-snail](https://steady-snail.tistory.com/67)
