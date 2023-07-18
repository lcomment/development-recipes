## 스코프 함수 (Scope Function)

> ### 스코프 함수란?

: 특정 객체의 컨텍스트 안에서 특정 동작(속성 초기화, 활용 등)을 실행하기 위한 목적만을 가진 함수

- Kotlin에서 기본적으로 `표준 스코프 함수` 제공
- 객체의 이름을 일일이 참조할 필요 없이 객체 접근 및 핸들링 가능
- `apply`, `run`, `with`, `also`, `let`

<br>

> ### 1. apply

```kotlin
inline fun <T> T.apply(block: T.() -> Unit): T {
    block()
    return this
}
```

- apply 스코프 내 모든 명령들이 실행하고 나면 명령들이 적용된 `새로 생성된 객체` 반환
- 확장 함수이기 때문에 객체 컨텍스트에서 recetiver(`this`) 사용 가능

```kotlin
fun main() {
    var coffee = Coffee("americano", 1500).apply {
        this.name = "Black $name"
        event()
    }

    println("${coffee.name} : ${coffee.price}원")
}

class Coffee(var name: String, var price: Int) {
    fun event() {
        price += 1000
    }
}
```

<br>

> ### 2. run

```kotlin
inline fun <T, R> T.run(block: T.() -> R): R {
    return block()
}
```

- run 스코프 내 `모든 명령들이 실행된 값` 반환
- 특정 객체의 프로퍼티를 출력하거나 계산값을 활용하는 등의 핸들링으로 활용
- 확장 함수이기 때문에 객체 컨텍스트에서 recetiver(`this`) 사용 가능
- safe call(`?.`)을 붙여 non-null일 때만 실행 가능

```kotlin
fun main() {
    var coffee = Coffee("americano", 1500).run {
        this.price + 3000
        // event() 함수를 호출하면 리턴값인 Unit이 출력됨
    }

    println("$coffee 원")
}

class Coffee(var name: String, var price: Int) {
    fun event() {
        price += 1000
    }
}
```

<br>

> ### 3. with

```kotlin
inline fun <T, R> with(receiver: T, block: T.() -> R): R {
    return receiver.block()
}
```

- 확장 함수 아님 → 컨텍스트 객체를 argument로 전달해야 receiver(`this`) 사용 가능
- 특징 및 동작은 `run`과 동일

```kotlin
fun main() {
    var coffee = Coffee("americano", 1500)

	with(coffee) {
        println("$name")
        println("$price 원")
    }
}

class Coffee(var name: String, var price: Int) {
    fun event() {
        price += 1000
    }
}
```

<br>

> ### 4. also

```kotlin
inline fun <T> T.also(block: (T) -> Unit): T {
    block(this)
    return this
}
```

- also 스코프에서 생성된 인스턴스 반환
- it을 사용해 프로퍼티 접근 가능
- 객체 프로퍼티를 전혀 사용하지 않거나 변경하지 않고 사용하는 경우 유용
- ex. 객체 데이터 validation 체크, 디버그, 로깅

<br>

> ### 5. let

```kotlin
inline fun<T, R> T.let(block: (T) -> R): R {
    return block(this)
}
```

- let 스코프 내 `모든 명령들이 실행된 값` 반환
- it을 사용해 프로퍼티 접근 가능
- safe call을 통해 nullable 객체를 다른 nullable 객체로 변환하는 경우 사용
- ex. JPA의 더티체킹을 활용한 update

```kotlin
fun update(request: GrapeUpdateRequest): Grape {
    request.name?.let {
        this.name = it.name
    }
    request.acidity?.let {
        this.acidity = it.acidity
        }
    request.body?.let {
        this.body = it.body
    }
    request.sweetness?.let {
        this.sweetness = it.sweetness
    }
    request.tannin?.let {
        this.tannin = it.tannin
    }

    return this
}
```
