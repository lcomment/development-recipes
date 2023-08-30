## 더티 체킹 (Dirty Checking)

&nbsp; 더티 체킹의 더티(`Dirty`)는 `상태의 변화`를 의미한다. 즉, 더티 체킹이란 `상태 변경 검사`를 의미한다. 이는 `Transaction` 안에서 엔티티의 변경이 일어나면 변경 내용을 DB에 반영하는 JPA의 특징이다.

```java
@Getter
@Entity(name = "users")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@EntityListeners(AuditingEntityListener.class)
public class User {
	@Id
	@Column(name = "user_id")
	private String id;

    @Column("nickname")
    private String nickname;

    @Column("age")
    private Integer age;

    @Embedded
	private AuditEntity auditEntity;

    @Builder
	public User(String id, String nickname, int age) {
		this.id = id;
		this.nickname = nickname;
        this.age = age;
		this.auditEntity = new AuditEntity();
	}

    public void updateNickname(String nickname) {
        this.nickname = nickname;
    }
}
```

&nbsp; 다음과 같은 User 엔티티가 있다고 가정하자. 우리는 `nickname` 컬럼을 업데이트 하고 싶을 때 Service 레이어에 다음과 같이 코드를 작성할 수 있다.

```java
. . .
    @Transactional
    public User updateNickname(String userId, String nickname) {
        User user = findById(userId);
        user.updateNickname(nickname);

        return user;
    }
. . .
```

&nbsp; 하지만 여기 문제가 하나 있다. 더티 체킹으로 생성되는 update query는 기본적으로 `모든 필드를 포함`한다. JPA는 모든 필드에 대해 미리 쿼리를 만들어놓고 `재사용`한다. 위와 같이 컬럼이 적은 상황에서는 상관 없지만, 컬럼이 많은 경우엔 query가 부담이 될 수 있다. 따라서 이럴 경우, 엔티티의 최상단에 `@DynamicUpdate` 어노테이션을 추가하여 변경되는 필드만 반영될 수 있도록 할 수 있다.
