## Given-When-Then 패턴

- ### `Given`
  - 테스트를 수행하기 전에 테스트에 필요한 `환경를 설정`하는 단계
  - 테스트에 필요한 변수를 정의
  - Mock 객체를 통해 특정 상황에 대한 행동 정의
- ### `When`
  - 테스트의 `목적`을 보여주는 단계
  - 실제 테스트 코드가 포함됨
  - 테스트를 통한 결과를 가져옴
- ### `Then`
  - 테스트의 `결과를 검증`하는 단계
  - 일반적으로 When 단계의 결과를 검증
  - 결과가 아니더라도 검증은 모두 Then 단계에 포함

```java
    . . .

    @Test
    void join() {
        // given: 이게 주어졌을 때
        Member member = new Member(1L, "memberA", Grade.VIP);

        // when: 이런 상황에서
        memberService.join(member);
        Member findMember = memberService.findMember(1L);

        // then: 이런 결괏값을 기대
        Assertions.assertThat(findMember).isEqualTo(member);
    }
    . . .
```

<br>

## F.I.R.S.T 전략

- ### `F`ast
  - `빠르게` 수행돼야 한다
  - 목적을 단순하게 설정해서 작성
  - 외부 환경을 사용하지 않는 단위테스트 작성
- ### `I`solated
  - 하나의 테스트 코드는 `하나의 목적`에 대해서만 수행돼야 한다
- ### `R`epeatable
  - 어떤 환경에서도 `반복 가능`하도록 작성해야 한다
  - 개발 환경의 변화나 네트워크의 연결 여부와 상관없이 수행돼야 함
- ### `S`elf-Validating
  - 그 자체만으로도 테스트의 검증이 완료돼야 한다
  - 테스트를 `성공`했는지 `실패`했는지 확인할 수 있는 코드가 있어야 함
  - 개발자가 직접 결괏값과 기댓값을 비교한다면 좋지 못한 테스트 코드임
- ### `T`imely
  - 테스트하려는 애플리케이션 코드를 `구현하기 전`에 테스트 코드가 완성돼야 한다
  - 너무 늦게 작성되면 정상적인 역할을 수행하지 못함
  - TDD가 아니라면 이 규칙은 `제외 가능`
