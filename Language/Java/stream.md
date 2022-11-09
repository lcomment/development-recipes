## Java의 스트림(stream)

&nbsp; 스트림(stream)은 `Java 8` 버전부터 람다와 함께 지원되기 시작한 기능이다. 이전까지 Java에서 데이터를 다룰 때 `for`문과 `Iterator`를 이용해왔다. 이러한 방식들은 코드 가독성과 재사용성이 떨어지고, 가장 큰 문제점은 데이터 소스마다 다른 방식으로 다뤄야 한다는 것이었다. Iterator 등을 이용해 표준화 했지만, `Collections.sort()`, `Arrays.sort()` 등 같은 기능의 중복된 메서드들은 여전히 불편하다. 이를 해결하기 위해 `stream`이 등장했다.

<br>

> ### 스트림(stream)

&nbsp; 스트림은 데이터 소스를 `추상화`하고, 데이터를 다루는데 자주 사용되는 `메서드들을 정의` 해놓다. 아래 코드와 같이 스트림을 사용하면 코드가 간결해져 이해하기 쉽고, 재사용성 또한 높다.

```java
String[] sArr = {"bbb", "ccc", "aaa"};
List<String> sList = Arrays.asList(sArr);

Stream<String> stream1 = Arrays.stream(sArr);
Stream<String> stream2 = sList.stream();

stream1.sorted.forEach(System.out::println);
stream2.sorted.forEach(System.out::println);
```

<br>

> ### 스트림은 데이터 소스를 `변경하지 않는다`.

→ stream은 데이터 소스로부터 데이터를 읽기만할 뿐, 데이터 소스를 변경하지는 않음

```java
List<String> sList = Arrays.asList({"bbb", "ccc", "aaa"});
Stream<String> stream = sList.stream();

// 정렬 결과를 새로운 List에 담아서 반환
List<String> sortedList = stream.sorted.collect(Collections.toList());
```

<br>

> ### 스트림은 `일회용`이다.

→ stream은 한번 사용하면 닫혀서 다시 사용할 수 없음

```java
List<String> sList = Arrays.asList({"bbb", "ccc", "aaa"});
Stream<String> stream = sList.stream();

stream.sorted.forEach(System.out::println);
int numOfStr = stream.count();  // Error: 스트림이 이미 닫혔음
```

<br>

> ### 스트림은 작업을 `내부 반복`으로 처리한다.

- 내부 반복: 반복문을 메서드 내부에 숨길 수 있음
- 스트림을 이용한 작업이 간결할 수 있는 비결

```java
List<String> sList = Arrays.asList({"bbb", "ccc", "aaa"});
for(String s : sList)
    System.out.println(s);

stream.forEach(System.out::println);
```

<br>

## 스트림의 연산

```java
stream.distinct().limit(5).sorted().forEach(System.out::println);
```

- `중간 연산`
  - 연산 결과가 스트림인 연산
  - 스트림에 연속해서 중간 연산할 수 있음
  - 위 예제의 distinct(), limit(), sorted()
- `최종 연산`
  - 연산 결과가 스트림이 아닌 연산
  - 스트림의 요소를 소모하므로 단 한번만 가능
  - forEach()

| **중간연산**                                                                                                                                                                                                                                                      | **설명**                  |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| Stream&lt;T> distinct()                                                                                                                                                                                                                                           | 중복 제거                 |
| Stream&lt;T> filter(Predicate&lt;T> predicate)                                                                                                                                                                                                                    | 조건에 안 맞는 요소 제외  |
| Stream&lt;T> limit(long maxSize)                                                                                                                                                                                                                                  | 스트림의 일부를 잘라냄    |
| Stream&lt;T> skip(Long n)                                                                                                                                                                                                                                         | 스트림의 일부를 건너뜀    |
| Stream&lt;T> peek(Consumer&lt;T> action)                                                                                                                                                                                                                          | 스트림의 요소에 작업 수행 |
| Stream&lt;T> sorted()<br>Stream&lt;T> sorted(Comparator&lt;T> comparator)                                                                                                                                                                                         | 스트림의 요소를 정렬      |
| Stream&lt;R> `map`(Function&lt;T, R> mapper)<br>DoubleStream `mapToDouble`(ToDoubleFunction&lt;T> mapper)<br>IntStream `mapToInt`(ToIntFunction&lt;T> mapper)<br>LongStream `mapToLong`(ToLongFunction&lt;T> mapper)                                              | 스트림의 요소를 반환      |
| Stream&lt;R> `flatMap`(Function&lt;T, Stream&lt;R>> mapper)<br>DoubleStream `flatMapToDouble`(Function&lt;T, DoubleStream> mapper)<br>IntStream `flatMapToInt`(Function&lt;T, IntStream> mapper)<br>LongStream `flatMapToLong`(Function&lt;T, LongStream> mapper) | 스트림의 요소를 반환      |

<br>

| **최종연산**                                                                                                                                                                                                    | **설명**                                                                                      |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| void forEach(Consumer&lt;? Super T> action)<br>void forEachOrdered(Consumer&lt;? Super T> action)                                                                                                               | 각 요소에 지정된 작업 수행                                                                    |
| long count()                                                                                                                                                                                                    | 스트림의 요소의 개수 반환                                                                     |
| Optional&lt;T> max(Comparator<? super T> comparator)<br>Optional&lt;T> min(Comparator<? super T> comparator)                                                                                                    | 스트림의 최대값/최소값 반환                                                                   |
| Optional&lt;T> findAny() `→ 아무거나 하나`<br>Optional&lt;T> findFirst() `→ 첫 번째 요소`                                                                                                                       | 스트림의 요소 하나를 반환                                                                     |
| boolean allMatch(Predicate&lt;T> p) `→ 모두 만족`<br>boolean anyMatch(Predicate&lt;T> p)`→ 하나라도 만족`<br>boolean noneMatch(Predicate&lt;> p)`→ 모두 만족 X`                                                 | 주어진 조건을 모든 요소가 만족시키는지, 만족시키지 않는지 확인                                |
| Object[ ] toArray()<br>A[ ] toArray(IntFunction&lt;A[ ]> generator)                                                                                                                                             | 스트림의 모든 요소를 배열로 반환                                                              |
| Optional&lt;T> `reduce`(BinaryOperator&lt;T> accumulator)<br>T `reduce`(T identity, BinaryOperator&lt;T> accumulator)<br>U `reduce`(U identity, BiFunction<U, T, U> accumulator, BinaryOperator&lt;U> combiner) | 스트림의 요소를 하나씩 줄여가면서 계산                                                        |
| R `collect`(Collector<T, A, R> collector)<br>R `collect`(Supplier&lt;R> supplier, BiConsumer<R, T> accumulator, BiConsumer<R, R> combiner)                                                                      | 스트림의 요소 수집<br>주로 요소를 그룹화하거나 분할한 결과를 컬렉션에 담아 반환하는 데에 사용 |

<br>

> ### 지연된 연산

- `최종연산`이 수행되기 전까지 `중간연산`이 수행되지 않음
- 최종연산이 수행돼야 스트림의 요소들이 중간연산을 거쳐 최종연산에서 소모됨

> ### Stream&lt;Integer&gt;와 IntStream

- 요소의 타입이 T인 스트림 → `Stream<T>`
- Autoboxing과 Unboxing으로 인한 비효율을 줄이기 위해 기본형으로 다루는 스트림 제공
  - IntStream, DoubleStream, LongStream
  - 더 효율적

> ### 병렬 스트림

- 스트림의 장점 중 하나 → `병렬 처리`가 쉽다
- 내부적으로 프레임워크를 이용해 자동으로 연산을 병렬 수행
- 그저 스트림에 `parallerl()` 메서드를 호출하면 됨

---

### 참고자료

- [자바의 정석](https://www.coupang.com/vp/products/57799011?itemId=200423111&vendorItemId=3137398432&src=1042503&spec=10304984&addtag=400&ctag=57799011&lptag=10304984I200423111V3137398432&itime=20221109144341&pageType=PRODUCT&pageValue=57799011&wPcid=16620354254556634629688&wRef=&wTime=20221109144341&redirect=landing&gclid=Cj0KCQiAmaibBhCAARIsAKUlaKRszeGg5n_biK1zeKVWMIEoQf8gNBaxEUdIAJChUuQNcwPtBtThyooaAr0kEALw_wcB&campaignid=18394378295&adgroupid=&isAddedCart=)
