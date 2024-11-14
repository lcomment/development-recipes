# JUnit5의 Extension

## Extension 인터페이스

- Extension을 통해 Test class, method 확장 가능
- 동작 방식은 테스트의 이벤트, Life Cycle에 관여하게 됨
- `ExtendWith`를 통해 Extension 구현체를 지정해 테스트를 실행하는 방법을 커스터마이징 할 수 있음

### TestInstancePostProcessor

```java
@FunctionalInterface
@API(
    status = Status.STABLE,
    since = "5.0"
)
public interface TestInstancePostProcessor extends Extension {
    void postProcessTestInstance(Object var1, ExtensionContext var2) throws Exception;
}
```

## Lifecycle Callbacks

### Lifecycle

- BeforeAllCallback
- BeforeAll
- BeforeEachCallback
- BeforeEach
- BeforeTestExecutionCallback
- Test
- AfterTestExecutionCallback
- AfterEach
- AfterEachCallback
- AfterAll
- AfterAllCallback


### Callbacks

- BeforeAllCallback
- BeforeEachCallback
- BeforeTestExecutionCallback
- AfterTestExecutionCallback
- AfterEachCallback
- AfterAllCallback


