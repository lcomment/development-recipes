## Data Class

> ### Data Class란?

: 데이터를 다루는 데 최적화된 클래스

- POJO 클래스로 구현된 데이터 클래스의 단점
  - 보일러 플레이트 코드가 필요
- 이런 고충을 해결하기 위해
  - Java → Record
  - Kotlin → Data Class
- Data Class 선언의 제한 사항
  - 기본 생성자에 `최소 하나`의 파라미터가 있어야 함
  - 기본 생성자의 파라미터는 val이나 var여야만 함
  - abstract, open, sealed, inner가 안 됨

> ### Canonical Methods

- Data Class는 기본적으로 캐노니컬 메서드 지원
- getter & setter, equals(), hashCode(), toString()

> ### copy()

: 원하는 파라미터를 Overriding 해서 데이터 클래스의 새로운 인스턴스를 `깊은 복사`하여 생성

```kotlin
val americano = Coffee("Americano", 4500)
val latte = americano.cope(name = "Latte")
```

<br>

> ### Destructuring

: 객체의 구조를 분해 후 할당이나 확장과 같은 연산을 수행

```kotlin
val americano = Coffee("Americano", "dark", 4500)
val (name, kind, price) = americano

// 사용 되지 않는 값은 언더바(_)로 대체
val latte = Coffee("Latte", "sweet", 5000)
val (name, _, price) = latte
```

<br>

> ### Reference

- [https://readystory.tistory.com/85](https://readystory.tistory.com/85)
