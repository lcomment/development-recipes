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

---

## 👨‍💻 사용 예시

```Java
public class ComputerTest {
    Computer computer;
    prviate static final int MAX_SIZE = 3;

    @BeforeEach
    void setUp() { computer = new Computer(); } // Test 실행 전 실행되어야 하는 메소드

    @Test
    @DisplayName("랜덤으로 뽑은 숫자 길이 확인하는 테스트")
    void 랜덤으로_뽑은_숫자가_MAX_SIZE와_일치하면_통과(){
        assertThat(computer.getNumbers().size())
            .as("숫자 길이가 3이 아닙니다.") // 실패 시, 출력하는 내용
            .isEqualTo(MAX_SIZE);
    }
}
```

추가로, 같은 테스트 메서드를 통해서 테스트 하고 싶은데, 매개 변수를 여러 개 집어 넣어 테스트 하고 싶을 수도 있을 것이다. 다음과 같은 방법을 이용해보자.

- 테스트 코드 설명
  abc, 122, a12, 133이 들어온다면 서로 다른 3개의 **_숫자_** 가 아니기 때문에 IllegalArgumentException을 던진다. 따라서, ValueSource에 인자값을 넣어 정확히 exception을 던지는지 테스트하는 것이다.

```Java
public class PlayerTest {
    Player player;
    CheckException check;

    @BeforeEach
    void setUp() {
        player = new Player();
        check = new CheckException();
    }

    @ParameterizedTest
    @DisplayName("잘못된 입력이 들어왔을 때 IllegalArgumentException 예외 테스트")
    @ValueSource(strings = {"abc", "122", "a12", "133", "1224", "a1", "012"})
    void testWithValueSource(String stringArg) {
        Assertions.assertThrows(IllegalArgumentException.class, () -> player.decideNumbers(stringArg));
    }
}

```

마지막으로, 메서드 Return값이 객체인 경우, 객체의 파라미터를 비교하여 올바른 값을 출력하는지 테스트 하고 싶을 수 있다. isEqualToComparingFieldByField 메서드는 deprecated되었기 때문에 다음과 같이 사용할 수 있다.

- 테스트 코드 설명

```Java
public class Result {
    int strike;
    int ball;

    public Result(int strike, int ball){
        this.strike = strike;
        this.ball = ball;
    }
}
```

다음과 같이 Result 클래스의 strike과 ball을 비교하고 싶을 때, 객체끼리 단순이 isEqualTo()를 통해 비교한다면 참조값이 서로 다르기 때문에 Fail이 될 수 밖에 없다. AssertJ 공식 문서에서는 다음과 같은 방향을 제시합니다.

- usingRecursiveComparison()을 활용

```Java
public class JudgeTest {
    Judge judge;

    @BeforeEach
    void setUp() {
        judge = new Judge();
    }

    @Test
    @DisplayName("정확한 strike과 ball개수를 세는지 테스트")
    void testWithResults1() {
        ArrayList<Integer> computer = new ArrayList<>(Arrays.asList(1, 2, 3));
        ArrayList<Integer> player = new ArrayList<>(Arrays.asList(2, 3, 1));
        assertThat(judge.compareBalls(player, computer))
                .as("실패시 -> 예상 값과 다르게 출력됨.")
                .usingRecursiveComparison()
                .isEqualTo(new Result(0, 3));
    }
}
```
