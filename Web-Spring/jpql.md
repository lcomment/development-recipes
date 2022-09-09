## JPQL (; Java Persistence Query Language)

: 엔티티 객체를 조회하는 객체지향 쿼리

- SQL과 비슷한 문법을 가지며, JPQL은 결국 `SQL로 변환`됨
- `검색 조건`이 포함된 SQL 사용에 어려움을 겪은 JPA의 문제 해결

### 특징

- 테이블이 아닌 `엔티티 객체`를 대상으로 쿼리
- SQL을 `추상화` 했기 때문에 특정 벤더에 `독립적`
- JPA는 JPQL을 분석하여 SQL을 생성한 후 DB에서 조회

<br>

---

### **참고자료**

- Book
  - [자바 ORM 표준 JPA, 김영한](http://www.yes24.com/Product/Goods/90439472)
