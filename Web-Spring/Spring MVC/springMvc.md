> ### [MVC Pattern에 대해 먼저 알아보자❗️](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/SW%20Engineering/mvc.md)

<br>

## Spring MVC Framework

&nbsp; Spring MVC는 Spring에서 제공하는 웹 모듈로, Model, View, Controller를 활용해 사용자의 다양한 `HTTP Request를 처리`하고, `다양한 형식의 Response`, `View를 리턴`하는 응답까지, 다양한 응답을 할 수 있도록 도와주는 프레임워크이다. 아래의 사진을 보며 Spring MVC의 작동원리에 대해 이해해보자.

<div align=center>
    <img src='../resources/spring/springMVC.png'>
    <H5>출처: https://mangkyu.tistory.com/m/95</H5>
</div>

1. Client가 URL을 통해 `Request를 보냄`
2. 프론트 컨트롤러(Front Controller)인 `DispatcherServlet`은 `Handler Mapping을 통해 해당 Request가 어느 컨트롤러에게 온 요청인지 찾음
3. DispatcherServlet이 `Handler Adapter`에게 Reqeust 전달을 넘김
4. Handler Adapter가 `Controller`에게 Request 전달
5. Controller는 `비즈니스 로직`을 처리한 후 `View Name`을 리턴
6. DispatcherServlet은 `View Resolver`를 통해 해당 View를 찾음
7. Controller에서 View에 전달할 데이터를 추가
8. 데이터가 추가된 View 리턴

<br>

## Spring MVC의 장점

- DI를 통해 컴포넌트 간의 결합도를 낮출 수 있음
- 단위 테스트 용이
- IoC를 통해 Bean의 라이프사이클에 대해 신경쓰지 않고 개발에 집중할 수 있음

<br>

---

### 참고자료

- [@kotlinworld](https://kotlinworld.com/m/326)
- [@mangkyu](https://mangkyu.tistory.com/m/95)
- [@gmlwjd9405](https://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html)
