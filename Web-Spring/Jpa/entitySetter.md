## Entity에 Setter를 지양해야 하는 이유

> ### 1. 사용 의도를 파악하기 어렵다

```java
Board board = new Board();

board.setId(1L);
board.setTitle("Test Title");
board.setContent("This is sample Board");
```

&nbsp; 위와 같이 setter를 사용했을 때, 저 코드만 봐서는 엔티티를 `생성`하는지, `수정`하는지 의도를 알 수 없다. 위와 같이 간단한 Entity에서는 불편함을 느끼지 못할 수 있지만, 내부가 복잡하면 복잡할수록 불편할 것이다.

<br>

> ### 2. 일관성을 유지하기 어렵다

```java
public Board update(Long id) {
    Board board = findById(id);

    board.setTitle("set title");
    board.setContent("set content");
    return board;
}
```

&nbsp; `public으로 작성된 setter`를 통해 어디서든 접근이 가능하기에 의도치 않게 Board 값이 변경되는 경우가 발생할 수 있다. 이렇게 되면 Board 객체의 일관성이 무너지게 된다.

<br>

## 그렇다면 어떻게 update를 구현할까?

```java
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Board {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	
	@Column(nullable = false)
	private String title;
	
	@Column(nullable = false)
	private String content;
	
	@Builder
	public Board(String title, String content) {
		this.title = title;
		this.content = content;
	}

    public Board update(String title, String content) {
        this.title = title;
        this.content = content;
        return this;
    }
}
```

&nbsp; JPA는 상태 변경 검사인 `Dirty Checking`이라는 특성을 가지고 있다. 따라서 위와 같이 `update()` 메서드를 Entity 내부에서 구현해주기만 하면 손쉽게 수정 기능을 구현할 수 있다.

&nbsp; 또 생성자를 통해 Entity의 `일관성`을 유지시켜 줘야 한다. 만약 column이 3개 이상이라면 `@Builder`를 사용하도록 하자.

---

### Reference

- [@langoustine](https://velog.io/@langoustine/setter%EB%A5%BC-%EC%93%B0%EC%A7%80%EB%A7%90%EB%9D%BC%EA%B3%A0)