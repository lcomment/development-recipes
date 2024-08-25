## 트랜잭션 스크립트 패턴

> ### Transaction Script Pattern

: 소프트웨어 설계에서 비즈니스 로직을 절차적으로 구현하는 패턴

- 애플리케이션의 비즈니스 로직을 독립된 스크립트(메서드)로 정의
- 스크립트는 특정 비즈니스를 처리하기 위한 모든 로직을 포함

> ### 예시 코드

```java
public class LikeService {

	public void likeCake(final User user, final Long cakeId) {
		final Cake cake = cakeRepository.findById(cakeId);
		final CakeLike cakeLike = cakeLikeRepository.findOrNullByUserAndCake(user, cake);

		likeOrCancel(cakeLike, user, cake);
	}

	private void likeOrCancel(final CakeLike cakeLike, final Cake cake, final User user) {
		if (isNull(cakeLike) {
			cakeLikeRepository.save(new CakeLike(cake, user));
		} else {
			cakeLikeRepository.delete(new CakeLike(cake, user));
		}
	}
}
```

> ### 장점

- 단순한 로직: 로직이 단순한 경우, 구현이 쉽고 유지보수가 간단함
- 명확한 비즈니스 로직: 각 메서드가 특정 비즈니스 트랜잭션을 명확하게 다룸
- 빠른 개발 속도: 초기 개발 단계에서 빠르게 비즈니스 로직 구현 가능

> ### 단점

- 힘든 유지보수: 비즈니스 로직이 복잡해지면 코드가 중복되거나 유지보수가 힘들어짐
- 재사용성 저하: 각 스크립트가 독립적으로 동작
- 부족한 도메인 지식: 도메인에 대하여 캡슐화하거나 표현하기 어려움

<br>

## 도메인 모델 패턴

> ### Domain Model Pattern

: 비즈니스 도메인 로직을 도메인 객체로 표현하는 설계 방법

- 엔티티, 값 객체, 애그리게이트 등의 객체 정의 및 비즈니스 로직을 객체에 캡슐화
- 응집도가 높은 도메인 객체를 만들고, 객체 간 결합도를 낮추는 유연한 설계 가능

> ### 예시 코드

```java
public class LikeService {
	
	public void likeCake(final User user, final Long cakeId) {
		final Cake cake = cakeRepository.findByIdWithLike(cakeId);

		if (!cake.isLikedBy(user)) {
			user.likeCake(cake);
		} else {
			user.unLikeCake(cake);
		}
	}
}
```

> ### 장점

- 비즈니스 로직 캡슐화: 코드가 도메인을 잘 반영하고 이해하기 쉬워짐
- 높은 재사용성: 도메인 객체들은 독립적이고 재사용이 용이
- 용이한 유지보수: 변화하는 요구사항에 맞추어 시스템 확장 및 유지보수가 쉬움
- 도메인 지식 반영: 비즈니스와 코드의 일관성 유지

> ### 단점

- 초기 설계 비용: 초기 단계에서 복잡도 증가
- 복잡성 관리: 시스템이 복잡해지면 도메인 모델 자체가 복잡해짐
- 오버엔지니어링: 단순한 애플리케이션에 적용 시, 과도한 설계로 인한 개발 비용 증가
