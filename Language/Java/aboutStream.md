# Java 8부터 제공되는 stream API 얼마나 알고 있나요?

## 🎈 왜 stream api를 사용하면 좋을까?

- 클린코드를 작성하는데 있어서 한 메서드에 오직 한 단계의 들여쓰기(indent)만 한다라는 말을 자주 들어봤을 것이다. 이게 무슨 말일까?

```Java
(예시)
public void getEventNumber() {
    for(int i = 0; i < 100; i++) {
        for(int j = 0; j < 100; j++) {
            System.out.println(i * j);
        }
    }
}
```

- 2중 for문의 첫 번째 Block 내부 for문의 두 번째 Block -> 이러한 경우, indent가 2라고 말할 수 있다.

## 💁‍♂️ 그래서 말하고 싶은 것이 뭔데?

> 협업하는 과정 속에서 코드를 작성하는 것은 글을 쓰는 것과 같다. 보기 좋으며 깔끔한 글을 봤을때 이해하기도 좋고 간결하면 얼마나 독해하는 시간이 효과적인가? `코드를 작성하는 것도 글이라고 생각하고 남이 봤을 때 독해의 어려움이 없도록 노력해보자.`

`단순히 2중 for문이 해당되는 것이 아니라 로직을 잘 쪼개서 결합도를 낮추자는 의미이다`

본격적으로 ...

## Stream이란?

Java8부터 지원하는 Stream은 컬렉션, 배열등에 대해 저장되어 있는 요소들을 하나씩 참조하며 반복적인 처리를 가능하게 해준다. 따라서 불필요한 for문과 이 안에서 일어나는 if문등의 분기처리를 쓰지 않고 깔끔하고 직관적인 코드로 볼 수 있다.

## Stream 특징

> 1. Stream은 데이터를 변경하지 않는다 -> 원본데이터로부터 데이터를 읽기만 할 뿐, `원본 데이터 자체를 변경하지 않는다.`
> 2. Stream은 일회용이기에 필요하다면 정렬된 결과를 컬렉션이나 배열에 담아 반환할 수 있다.
> 3. Stream은 작업을 내부 반복으로 처리한다. 내부 반복이 무슨 뜻일까? 반복문을 메서드 내부에 숨길 수 있다는 것이다. 즉, `코드로 노출되지 않는다.`

## Stream 구조

크게 3가지
`Stream생성().중개연산().최종연산();` 의 형태를 가지고 있다.

Stream은 다양한 데이터소스에서 생성할 수 있는데

> 컬렉션, 배열, 가변 매개변수, 람다 표현식, 빈 스트림 등등이 있다.

## 중개 연산

중개 연산은 Stream을 전달 받아 Stream으로 반환하므로 중개 연산을 연속으로 사용할 수 있다. 또한 Stream의 중개 연산은 필터-맵(filter-map)기반의 API를 사용함으로 lazy 연산을 통해 성능을 최적화할 수 있다.

### ☑️ 대표적인 중개 연산

1. Stream 필터링: filter(), distinct()
2. Stream 변환: map(), flatMap()
3. Stream 제한: limit(), skip()
4. Stream 정렬: sorted()
5. Stream 연산 결과 확인: peek()

**_위에서 언급한 `lazy연산을 이해`하고 넘어가는 것이 좋다_**

Lazy 와 Eager, 지연 연산과 즉시 연산이라고 볼 수 있는데 조금 더 쉽게 말해보자면 Lazy는 코드가 주어졌을 때 곧바로 해당 코드를 실행하는 것이 아니라 실행결과가 필요한 시점에 실행하도록 하는 것이다.
다만 이러한 방식으로 코드가 동작하기 위해서는 준비 작업이 필요하므로 무조건 효율적이라고 보기는 어렵다.
특정 작업이 신뢰있으며 향후에 확실히 필요한 코드라면 미리 실행해 두는 것이 성능적으로 우월할수도 있다.<br><br>

```Java
public class Test {
    private int value;

    public Test(int value) {
        this.value = value;
    }

    public void printResultA() {
        System.out.printf("데이터 %d작업A", value);
    }

    public void printResultB() {
        System.out.printf("데이터 %d작업B", value);
    }

    public void printResultC() {
        System.out.printf("데이터 %d작업C", value);
    }

    @Override
    public String toString() {
        return "Data{" + value + `}`;
    }
}

// 결과 값을 예측해보자!
Stream.of(new Test(1), new Test(2), new Test(3))
        .peek(Test::printResultA)
        .peek(Test::printResultB)
        .forEach(Test::printResultC)
```

```Java
데이터 1의작업A
데이터 2의작업A
데이터 3의작업A
데이터 1의작업B
데이터 2의작업B
데이터 3의작업B
데이터 1의작업C
데이터 2의작업C
데이터 3의작업C
//이렇게 예측했다면 잘못됐다.
```

위처럼, 예측했다면 peek() 중개연산을 `3번씩 총 2번, 6번 호출`한 것으로 이해할 수 있다. <br>
**_Lazy연산은 준비과정을 가진다고 했다_**
`JVM은 최소한의 필수적인 작업만을 수행하고자, Lazy연산을 위한 준비작업을 수행한다.` 우선적으로 스트림 파이프라인이 어떠한 중간연산과 최종연산으로 구성되어있는지에 대해 검사한다. 이러한 검사를 바탕으로 최적화를 표현할 수 있는 것이다.

위에서의 연산은 peek() 중개 연산이 2개 있으니까 하나의 연산과정으로 병합하는 준비 과정이 진행된다.<br><br>

## 중개 연산의 return은 Stream<T>, 최종연산은 void이다.<br>

결과적으로, `peek() 중개 연산 2개를 하나의 과정으로 병합하여 준비한다.`
따라서, 최적화의 의미는 접근했던 stream중개 연산 6번을 `3번의 접근`으로 성능을 향상시킬 수 있다.

## 그렇다면 출력 결과는?

```
데이터 1의작업A
데이터 1의작업B
데이터 1의작업C
데이터 2의작업A
데이터 2의작업B
데이터 2의작업C
데이터 3의작업A
데이터 3의작업B
데이터 3의작업C
```

### 그동안의 코드 작성을 돌아보며 ...

> 필자는 Stream을 정확히 이해하기 전에 Java의 입력?에 대해 스트림을 사용하거나 구글링하여 검색해서 for문을 복잡하게 사용하거나 로직이 복잡해질 때, 해결하고자 하는 사항을 검색해서 사용하기만 했다.

> 이번 정리를 통해서 내부적인 로직을 이해할 수 있었고 코드의 Indent를 줄이며, 가독성 좋은 코드를 작성할 수 있도록 노력할 필요가 있다고 생각한다.

[Stream API 참고 링크](https://ahndding.tistory.com/23)<br>
[Lazy와 Eager참고 링크](https://bugoverdose.github.io/development/stream-lazy-evaluation/)
