## DAO (; Data Access Object)

- 데이터베이스의 데이터에 접근하기 위한 객체
- DB에 접근하기 위한 로직 및 비즈니스 로직을 분리하기 위해 사용

```java
@Repository
@RequiredArgsConstructor
public class MemberRepository {

    private final EntityManager em;

    public void save(Member member) {
        em.persist(member);
    }

    public Member findOne(Long id) {
        return em.find(Member.class, id);
    }

    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class).getResultList();
    }

    public List<Member> findByName(String name) {
        return em.createQuery("select m from Member m where m.name =:name", Member.class)
                .setParameter("name", name)
                .getResultList();
    }
}
```

<br>

## DTO

- 계층 간 데이터 교환을 하기 위해 사용하는 객체
- 로직을 가지지 않는 순수한 데이터 객체 (getter와 setter를 가짐)

```java
@Setter
@Getter
@AllArgsConstructor
public class PersonDTO {
    private String name;
    private int age;
}
```

<br>

## VO

- 값 오브젝트로써 값을 위해 쓰임
- `read-only` 특징을 가짐
