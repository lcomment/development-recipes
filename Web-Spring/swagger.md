<br>

- [❗️ Swagger](#swagger)
- 🏃 Swagger 설정
  - [🤝 Dependency 설정](#swagger-dependency-추가)
  - [🧭 Configuration 추가](#config-클래스-추가)
- [✍️ Swagger의 Annotation](#swagger-annotation)
- [🎨 Swagger UI 화면 커스텀](#swagger-ui-api-화면-커스텀)
- [💡 Swagger 연동 오류](#💡swagger-연동-오류)

<br>

## Swagger

: OAS(Open Api Specification)를 위한 프레임워크

- API 문서화를 쉽게 할 수 있도록 도와줌
- 파라미터를 넣어 Request-Response 테스트 가능
- 협업에 용이

<br>

## Swagger Dependency 추가

> ### 1) Gradle

```yaml
implementation "io.springfox:springfox-boot-starter:3.0.0"
implementation "io.springfox:springfox-swagger-ui:3.0.0"
```

> ### 2) Maven

```xml
<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-boot-starter</artifactId>
			<version>3.0.0</version>
</dependency>
<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger-ui</artifactId>
			<version>3.0.0</version>
</dependency>
```

<br>

## Config 클래스 추가

```java
@Configuration
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.OAS_30)
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any())
                .build();
    }
}
```

- **`Docket`**: Swagger 설정의 핵심이 되는 Bean
- **`useDefaultResponseMessages`**: Swagger 에서 제공해주는 기본 응답 코드 (200, 401, 403, 404). true 로 설정하면 기본 응답 코드를 노출
- **`apis`**: api 스펙이 작성되어 있는 패키지 (Controller) 를 지정
- **`paths`**: apis 에 있는 API 중 특정 path 를 선택

<br>

## Swagger Annotation

- `@ApiOperation()`
  - Controller 안의 Method 설명 추가
  - value, notes
- `@ApiImplicitParam()`
  - 해당 API Method 호출에 필요한 Parameter들의 설명 추가
  - name, value, required, dataType, paramType, defaultValue
    - paramType은 query, path
  - 파라미터가 복수개일 경우
    - @ApiImplictParam`s` 안에 @ApiImplictParam 복수개 사용
- `@ApiResponse()`
  - 해당 Method의 Response에 대한 설명 작성
  - code, message
  - 여러 응답 명세
    - @ApiResponse`s` 안에 @ApiResponse 복수개 사용
- `@ApiParam()`
  - DTO의 field에 대한 설명 추가
  - value, required
- `@ApiModelProperty()`
  - DTO Class Field에 추가하면 해당 DTO Field의 예제 Data 추가
  - name, example
- `@ApiIgnore()`
  - Swagger UI 상 무시

<br>

## Swagger UI API 화면 커스텀

→ `.apiInfo(ApiInfo Type)` 작성

```java
@Configuration
public class SwaggerConfig{
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.OAS_30)
                .consumes(getConsumeContentTypes())
                .produces(getProduceContentTypes())
                .useDefaultResponseMessages(false)
                .apiInfo(getApiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.bng.ddaja"))
                .paths(PathSelectors.ant("/**"))
                .build();
    }

		. . .

    private ApiInfo getApiInfo() {
        return new ApiInfoBuilder()
                .title("**프로젝트 API 명세서")
                .description("**프로젝트 API 명세서입니다")
                .contact(new Contact("[lcomment]", "https://github.com/lcomment", "BNG"))
                .version("1.0")
                .build();
    }
}
```

<br>

## 💡Swagger 연동 오류

&nbsp; Spring boot 2.6 버전 이후 spring.mvc.pathmatch.matching-strategy 값이 `ant_apth_matcher`에서 `path_pattern_parser`로 변경되면서 몇몇 라이브러리(swagger 포함)에 오류가 발생할 수 있다. 이럴 경우에는 `application.properties` 또는 `application.yml`에 아래 설정을 추가하면 된다.

```yaml
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher
```
