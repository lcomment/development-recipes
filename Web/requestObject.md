## Request 객체의 3가지 메서드

> ### Param

- 주소에서 포함된 변수
- http://localhost:8080/user/profile/1 에서 `1`
- Spring → `@PathVariable`
- Nest js → `@Param`

<br>

> ### Query

- 주소 끝, ?뒤에 있는 변수
- http://localhost:8080/user?id=1&name=lcomment 에서 `id`와 `name`
- Spring → `@RequestParam`
- Nest js → `@Query`

<br>

> ### Body

- Json, XML 등의 데이터, 주소에서 알 수 없음
- 툴을 활용하면 알 수 있기 때문에 민감한 정보는 암호화 필요
- Spring → `@RequestBody`
- Nest js → `@Body`
