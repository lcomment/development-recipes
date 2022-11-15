## 트랜잭션 (Transaction)

: 데이터베이스의 상태를 변화시키기 위해 수행하는 작업의 단위

- 상태 변화 → SQL을 이용해 DB에 접근하는 것
  - SELECT
  - INSERT
  - UPDATE
  - DELETE
- ex) A가 B에게 10,000원 송금
  - A의 계좌에서 10,000원 `출금`
  - B의 계좌에 10,000원 `입금`
  - 두 동작 중 하나라도 정상 동작하지 않으면 문제 발생❗️
- 트랜잭션은 위와 같은 상황이 일어나지 않도록 보장
  - 두 동작 모두 성공 → `commit`
  - 하나라도 실패 → `rollback`

<br>

## ACID

→ 데이터의 유효성을 보장하기 위한 트랜잭션의 특징들

> ### Atomicity (원자성)

: 모든 작업이 `반영`되거나 모두 `롤백`되는 특성

<br>

> ### Consistency (일관성)

: 데이터는 `미리 정의된 규칙`에서만 수정이 가능한 특성

<br>

> ### Isolation (고립성)

: 둘 이상의 트랜잭션이 실행되고 있을 때, 트랜잭션 서로의 연선에 끼어들 수 없는 특성

<br>

> ### Durability (영구성)

: 한번 커밋된 트랜잭션의 내용은 영원히 적용되는 특성

<br>

---

### Reference

- [@mommoo](https://mommoo.tistory.com/62)
- [@chrisjune](https://chrisjune-13837.medium.com/db-transaction-%EA%B3%BC-acid%EB%9E%80-45a785403f9e)
