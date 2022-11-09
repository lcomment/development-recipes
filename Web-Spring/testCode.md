[참고링크](https://effortguy.tistory.com/113)<br>

# JUnit

JUnit이란 테스팅 프레임워크입니다.

JUnit 5는 이전 버전들과 다르게 3개의 서브 프로젝트 모듈로 이루어져있습니다.

JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage

JUnit Platform : TestEngine API, Console Launcher, JUnit 4 based Runner 등 포함
JUnit Jupiter : TestEngine API 구현체로 JUnit 5 구현
JUnit Vintage : TestEngine API 구현체로 JUnit 3, 4 구현

JUnit 5에서 JUnit Vintage 모듈을 포함하고 있어 JUnit 3, 4 문법을 사용할 수 있습니다. 하지만 완벽하게 지원해주는 것이 아니기 때문에 만약 사용한다하면 추가로 작업이 필요합니다.

---

## JUnit5 어노테이션

JUnit 5에서 테스트를 위해 다양한 어토네이션을 제공하는데, 아래에 있는 어노테이션만 알아도 테스트가 어느정도 가능하다.
|어노테이션|설명|
|---|--------|
|@Test|테스트 메서드를 나타내는 어노테이션으로 팔수로 작성되어야 함.|
|@BeforEach|각 테스트 메서드 시작 전에 실행되어야 하는 메소드에 써줌.|
|@AfterEach|각 테스트 메서드 종료 후에 실행되어야 하는 메소드에 써줌.|
|@BeforeAll|테스트 시작 전에 실행되어야 하는 메서드에 써줌.(static 메서드여야만 함)|
|@AfterAll|테스트 종료 후에 실행되어야 하는 메서드에 써줌.(static 메서드여야만 함)|
|@Disabled|실행되지 않아야 하는 테스트 메서드에 써줌|
|@DisplayName|Test Results에 나오는 테스트 클래스, 메서드 이름을 정할 수 있음.|

---

## JUnit 5 Assertions

Assertion이 한글 뜻으로 주장이라는 뜻이기에 테스타가 원하는 결과를 제대로 리턴하는지 에러는 발생하지 않는지 확인할 때 사용하는 메서드이다.

| 메서드                    | 설명                                             |
| ------------------------- | ------------------------------------------------ |
| fail                      | 무조건 실패 (레거시에 사용하면 좋다.)            |
| assertTrue                | 조건이 성공이면 True                             |
| assertFalse               | 조건이 실패면 True                               |
| assertNull                | 조건이 Null이면 True                             |
| aseertNotNull             | 조건이 Not Null이면 True                         |
| assertEquals              | expected와 actual이 동일하면 True                |
| assertArrayEquals         | 두 Array가 동일하면 True                         |
| assertIterableEquals      | 두 Iterable이 동일하면 True                      |
| assertLinesMatch          | 두 Stream이 동일하면 True                        |
| assertNotEquals           | expected와 actual이 다르면 True                  |
| assertSame                | 동일한 Object면 True                             |
| assertNotSame             | 다른 Object면 True                               |
| assertAll                 | 여러 Assertion이 True면 True                     |
| assertThrows              | 예상한 에러가 발생하면 True                      |
| assertDoesNotThrow        | 에러가 발생하지 않으면 True                      |
| assertTimeout             | 테스트가 지정한 시간보다 오래 걸리지 않으면 True |
| assertTimeoutPreemptively | 테스트가 지정한 시간보다 오래 걸리지 않으면 True |
