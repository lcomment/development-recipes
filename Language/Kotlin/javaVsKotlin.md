# Java와 Kotlin의 차이

## 1. Java에는 있지만 Kotlin에는 없는 기능

> ### Checked Exception

- ### Java
  - Throwable : 예외 계층의 최상위 클래스
  - Error
    - 시스템에서 발생한 비정상적인 에러
    - ex. StackOverFlowError
  - Exception
    - 시스템에서 포착 가능한 에러, 복구 가능
    - ex. IOExcpeion
  - RuntimeException
    - 런타임 시 발생하는 예외
    - ex. NullPointerException
  - Checked Exception은 컴파일 에러가 발생하기 때문에 예외 처리를 해줘야 함
  - 대부분의 개발자들이 의미 없는 예외 처리를 흔하게 하고 있음
- ### Kotlin
  - Checked Exception을 강제하지 않음

<br>

> ### 자료형

- Java
  - Primitive Type과 Reference Type 지원
- Kotlin
  - Reference Type만 지원
  - 최적화된 방식으로 컴파일

<br>

> ### 정적 멤버

- Java
  - static 키워드로 선언
- Kotlin
  - `companion object`로 대체

<br>

> ### 삼항 연산자

- Java
  - 존재
- Kotlin
  - if-else로 대체
  - val sound: String = if("tiger" == animal) "A" else "B"

<br>

> ### 세미콜론

Java는 필수지만, Kotlin은 선택.

<br>

## 2. Kotlin에는 있지만 Java에는 없는 기능

> ### 확장

- 개발자가 임의로 객체의 함수나 Property를 확장해서 사용 가능

```kotlin
fun String.first(): Char {
    return this[0];
}

fun String.addFirst(char: Char): String {
    return char + this.substring(0);
}

fun main() {
    println("ABCD".first())         // A
    println("ABCD".addFirst('Z'))   // ZABCD
}
```

<br>

> ### 데이터 클래스

- Kotlin
  - 데이터를 보관하거나 전달 목적을 가진 불변 객체 사용
  - DTO
- Java
  - lombok 사용
  - JDK 15 → record 추가

```kotlin
data class Person(val name: String, vla age: Int)
// hashCode(), equals(), toString() 자동생성
// 이 외에도 copy(), componentN()도 유용하게 사용 가능
```

<br>

> ### 문자열 템플릿

- kotlin
  - 문자열에 변수를 사용하거나 여러 행으로 된 텍스트 블록 생성 가능
  - Dynamic Query 가능
- java
  - JDK 13 → 텍스트 블록 지원

<br>

> ### Null 안정성

- Kotlin
  - `언어적 차원`에서 NPE가 발생할 가능성 제거
  - `?`, `!!`
  - `Nullable` 참조 → 컴파일 단계에서 Null 안정성 제공
- Java
  - JDK 8 → Optional 지원
  - 객체 생성에 따른 오버헤드 발생 및 컴파일 단계에서 Null 가능성 검사 X

<br>

> ### 그 외

- 스마트 캐스트
  - 개발자가 굳이 원하는 타입으로 캐스팅 하지 않아도 컴파일러가 알아서 캐스팅
- 실드 클래스
  - 미리 만들어 놓은 자료형들을 묶어서 제공
  - 열거형 클래스의 확장으로도 볼 수 있음
- 위임
  - `by` clause는 b 객체가 위임 객체 내부에 저장될 것임을 알려주며 컴파일러는 B의 모든 메서드를 b의 것을 활용하여 생성한다
- 중위 표현식
  - 호출자인 `.`과 파라미터 괄호를 생략하고, 함수명으로만 호출할 수 있는 Kotlin에서 제공해주는 표현식
- 연산자 오버로딩
  - `operator` 키워드로 연산자 오버로딩
- 코루틴
  - 동시성 프로그래밍 개념을 Kotlin에 도입한 것
