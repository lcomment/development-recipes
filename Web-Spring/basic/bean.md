## 빈 (Bean)

- `IoC` 컨테이너가 관리하는 객체
- 특정한 정보 등을 가지고 있는 클래스를 표현하는 `규칙`을 지닌 클래스

> ### 규약

- 반드시 클래스는 `패키지화` 돼야 함
- 멤버 변수는 `Property`라고 함
- 멤버 변수는 `private`로 지정
    - 외부 접근을 위한 getter와 setter 메서드 정의
- getter와 setter는 `public`으로 지정

<br>

## 빈 등록 방법

> ### xml 등록

- `<bean id="id 값" class="등록할 클래스">` 
    - 빈 등록
- `<property name="매개변수 이름" ref="id 값">`
    - 해당 빈의 Property를 등록하여 참조
- Main Application에서 `GenericXmlApplicationContext` 사용

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<!-- 빈 등록 -->
	<bean id="x" class="com.service.DeptServiceImpl">
		<property name="DAO" ref="y"></property>
	</bean>
	
	<bean id="y" class="com.dao.DeptDAO"></bean>

</beans>
```

<br>

> ### spring 2.5에서 추가된 기능

- `<context:component-scan base-package="패키지">`
    - 패키지의 각각 클래스에 `어노테이션`을 붙여 빈 등록
    - `,`를 통해 여러 개의 패키지 등록 가능
- `<context:exclude-filter type="annotation" 
        expression="패키지"/>`
    - 빈 등록에서 제외하고 싶은 어노테이션 명시
- `<context:include-filter type="annotation" 
        expression="패키지"/>`'
    - 특정 어노테이션 명시
<br>

> ### Java 파일 안에서 설정

- `@Configuration`
    - 해당 클래스가 `xml을 대체`하는 설정 파일임을 명시
- `@ComponentScan`
    - basePackages 설정
- Main Application에서 `AnnotationConfigApplicationContext` 사용

```java
@Configuration
public class ApplicationConfig {
    @Bean
    public DeptRepository deptRepository() {
        return new DeptRepository
    }

    @Bean 
    public DeptService deptService(DeptRepository deptRepository) {
        DeptService deptService = new DeptService();

        deptService.setDeptRepository(deptRepository);
        return deptService;

        . . .
    }
}
```

<br>

> ### Spring Boot

- `@SpringBootApplication`
    - `@ComponentScan`과 `@Configuration`이 내포돼 있음
    - 빈 등록과 의존성 주입을 자동으로 해줌

```java
@SpringBootApplication
public class Application {
 
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

<br>

---

### References

- [@coreminw](https://velog.io/@coreminw/Java-Bean%EC%9D%B4%EB%9E%80)
- [@wordbe](https://wordbe.tistory.com/entry/Spring-IoC-%EB%B9%88-%EB%93%B1%EB%A1%9D-%EB%B0%A9%EB%B2%95-5%EA%B0%80%EC%A7%80)