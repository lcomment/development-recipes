## Web.xml

- 설정을 위한 파일
- WAS가 최초로 구동될 때 각종 `설정 정의`
- 여러 `xml 파일`을 인식하도록 각 파일을 가리켜 줌
- `ContextLoaderListener`를 이용하여 `root-context` 생성
- `DispatcherServlet`을 이용하여 `servlet-context` 생성

<div align=center>
    <img src='../../resources/spring/basic/context.png' width=550>
</div>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

	<!-- 모든 서블릿과 필터에서 공유하는 Root Spring 컨테이너에 대한 설정 정의 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
	
	<!-- 모든 서블릿과 필터를 공유하는 Spring 컨테이너 생성 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- Application 요청 처리 -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

</web-app>
```

<br>

## root-context.xml

- Web 환경과 독립적인 객체 정의 → `비지니스 로직`
- root-context에 등록된 빈들은 모든 Context에서 사용 가능 → `공유`
- `Service`, `Repository` 등

```xml
<context:component-scan base-package="패키지경로">
	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	<context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
	<context:include-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
</context:component-scan>
```

<br>

## servlet-context.xml

- `Request`와 관련된 객체 정의
- 독립적인 Context를 가짐
- root-context 내 빈 사용 가능
- `Controller`, `ViewResolver`, `Interceptor` 등

```xml
<context:component-scan base-package="패키지경로">
	<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>
	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
</context:component-scan>
```

<br>

---

### References

- [@thiago6](https://thiago6.tistory.com/70)
- [@kingofbackend](https://kingofbackend.tistory.com/77#%EC%-A%A-%ED%--%--%EB%A-%--%EC%-D%--%--%EA%B-%--%EC%--%-D%--%EA%B-%B-%EB%B-%--%EC%A-%--%EC%-D%B-%EC%A-%--%EB%A-%-C%--%EC%--%--%EC%B-%AD%--%ED%--%B-%EA%B-%--%EB%A-%B-%EB%-D%--%--%EB%B-%--%EB%B-%--%EC%-D%B-%--WebApplicationContext%EC%--%--%--ApplicationContext%EC%-D%--%--%EA%B-%--%EA%B-%--%-C%--web-xml%EC%--%--%EC%--%-C%EC%-D%--%--servlet-context%EC%--%--%--root-context%EC%-D%--%--%EA%B-%--%EA%B-%--%--%EA%B-%B-%EB%A-%AC%EA%B-%A-%--ContextLoaderListenter%EC%--%--%--dispatcher-servlet%EC%-D%--%--%EA%B-%--%EA%B-%--%--%EC%B-%-D%--%EC%-D%B-%---%EA%B-%-C%EC%-D%--%--%EA%B-%--%EA%B-%--%EA%B-%--%--%EB%B-%BC%EB%--%-C%EB%A-%--%EB%-B%A-%--%ED%--%B-%EA%B-%--%EB%A-%B-%EC%-A%B-%EB%-B%--%EB%-B%A--%C-%A-)