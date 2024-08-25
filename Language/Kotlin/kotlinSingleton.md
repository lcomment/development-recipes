## Kotlin의 싱글톤

&nbsp; Kotlin에는 `static` 키워드 대신 `object`와 `companion object`를 지원한다.

> ### object

: 싱글톤 패턴을 위해 Kotlin이 지원하는 키워드로, 자동적으로 싱글톤 패턴이 구현된 클래스가 작성됨

- 단일 인스턴스 보장
  - object로 선언된 클래스는 애플리케이션 전체에서 하나의 인스턴스만 존재
  - 인스턴스는 프로그램이 실행될 때 자동으로 생성됩니다.
- 전역 상태 관리
  - 전역 상태나 설정 정보를 관리할 때 유용
  - 모든 곳에서 동일한 상태나 설정을 참조해야 할 경우 활용
- 동시성 문제 해결
  - 싱글톤 패턴을 직접 구현할 때 발생할 수 있는 동시성 문제 해결
  - Kotlin이 자체적으로 스레드 안전성(thread safe)을 보장

```kotlin
object Logger {
    fun log(message: String) {
        println("Log: $message")
    }
}

// 활용
Logger.log("This is a log message.")
```

> ### companion object

: 특정 클래스에 종속된 정적 멤버(static member)를 정의하기 위해 클래스 내부에 선언

- 정적 멤버 구현
  - 인스턴스를 생성하지 않고도 클래스 내부의 메서드나 변수를 사용 가능
  - 클래스 관련 유틸리티 함수나 상수를 정의할 때 유용
- 팩토리 메서드 패턴 구현
  - 객체 생성 로직을 캡슐화하기 위해 팩토리 메서드를 companion object에 정의
- 객체의 확장 가능성
  - 다른 클래스나 인터페이스를 상속하지 않고도, companion object에 확장 함수나 속성 추가 가능

```kotlin
class Student {
    companion object {
        const val SCHOOL = "KOTLIN SCHOOL"

        fun create(): MyClass {
            return Student()
        }
    }
}

// 사용
println(Student.SCHOOL)
val instance = Student.create()
```

→ companion object는 객체 내에 객체가 생기기 때문에 java의 static과 동일하게 사용하고 싶다면 @JvmField나 @JvmStatic을 활용하자.
