## RDB에서 꼭 지켜야 할 7가지 네이밍 규칙

> ### 공통

1. 이름은 `snake case`를 따른다.
   - userLogin, Name (X) → user_login, name (O)
2. `prefix`와 `postfix`는 사용하지 않는다.
   - user_TB (X)

<br>

> ### Table 관련

3. 테이블의 이름은 복수가 아닌 `단수`로 쓴다.
   - users (X) → user(O)
4. 가능하면 단어를 줄여쓰지 않는다.
   - mid_name (X) → middle_name (O)

<br>

> ### Attribute 관련

5. 테이블이 하나의 기본키를 가진다면, 그 속성의 이름은 `id`로 한다.
   - user_id (X) → id (O)
6. 외래키는 테이블 이름과 속성 이름을 `더해 정한다`.
   - user 테이블의 id → id
7. index와 constraint는 `descriptive`하게 작성한다.
   - 예를 들어, index는 테이블명, 속성명, 인덱스 유형이 포함돼야 한다.
   - user_xx (X) → user_xx_email_lower (O)

<br>

---

### 참고자료

- [@killu](https://killu.tistory.com/52)
