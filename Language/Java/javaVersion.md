## Java 버전별 차이 및 특징

> ### Java 8

- 기존 기업의 많은 프로젝트에서 사용
- `Lambda`와 `Stream`의 등장
- `Interface Default Method`
- `Optional`
- `new Date and Time API` (LocalDateTime 등)

<br>

> ### Java 9

- 모듈 시스템 등장
- Collections → List, Set, Map을 쉽게 구성할 수 있는 몇 가지 기능 추가
- Stream → takeWhile, dropWhile, iterate 메서드의 형태로 몇 가지 추가 기능
  - takeWhile() : Predicate 적용
  - dropWhile() : Predicate의 반대로 적용
- Optional → ifPresentOrElse() 추가 기능
  - ifPresentOrElse() : ifPresent()에 Empty Action 추가
- Interface → private method 사용 가능

```java

// Collections
List<String> list = List.of(*"one"*, *"two"*, *"three"*);

// stream
Stream<String> stream = Stream.iterate(*""*, s- > s + *"s"*)
        .takeWhile(s -> s.length() < 10);
```

<br>

> ### Java 10

- `var` 키워드 → 타입 추론
- `병렬 처리 GC`(Garbage Collection) 도입으로 인한 성능 향상
- JVM 힙 영역을 시스템 메모리가 아닌 다른 종류의 메모리에도 할당 가능

<Br>

> ### Java 11

- Oracle JDK와 OpenJDK 통합
- Oracle JDK가 구독형 유료 모델로 전환
- Lambda 지역변수 사용법 변경 → 지역변수로 var 사용 가능

<br>

> ### Java 12

- 유니코드 11 지원

<br>

> ### Java 13

- `새로운 Switch 표현식`
  - 값 반환 가능
  - fall-through/break 문제 없이 람다 스타일 구문 사용
- 유니코드 12.1 지원

```java
private void switchExample(int number) {
    return switch (number) {
        case 1, 2 -> "one or two";
        case 3, 4 -> "three or four";
        case 5, 6 -> "five or six";
        default -> {
            System.out.println("No matching cases");
            yield "unknown value";
        }
    };
}
```

<br>

> ### Java 14

- Switch 표현시 표준화
- instanceof 패턴 매칭
- `NullPointerExceptions` → 정확히 어떤 변수가 null인지 설명
- `Record`(data object) 선언 기능 추가 Preview

<br>

> ### Java 15

- Text-Blocks / Multiline Strings
- Record와 Pattern Matching 2차 Preview
- 스케일링 가능한 낮은 지연의 GC 추가 (`ZGC`)

```java
// Text-Blocks
String output = """
    Name: %s
    Phone: %s
    Address: %s
    Salary: $%.2f
    """.formatted(name, phone, address, salary)
```

<br>

> ### Java 16

- Pattern Matching for instanceof
- Unix-Domain Socket Channels
- Records와 Pattern Matching

<br>

> ### Java 17

- Pattern Matching for switch
- Deprecating the Security Manager

<br>

---

### Reference

- [@ljo_0920](https://velog.io/@ljo_0920/java-버전별-차이-특징)
