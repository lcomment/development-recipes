# SpringExtension

```java
public class SpringExtension implements BeforeAllCallback, AfterAllCallback, TestInstancePostProcessor, BeforeEachCallback, AfterEachCallback, BeforeTestExecutionCallback, AfterTestExecutionCallback, ParameterResolver {
	// 생략

	public void postProcessTestInstance(Object testInstance, ExtensionContext context) throws Exception {
		this.validateAutowiredConfig(context);
		getTestContextManager(context).prepareTestInstance(testInstance);
	}

	private void validateAutowiredConfig(ExtensionContext context) {
		ExtensionContext.Store store = context.getStore(AUTOWIRED_VALIDATION_NAMESPACE);
		String errorMessage = (String)store.getOrComputeIfAbsent(context.getRequiredTestClass(), (testClass) -> {
			Method[] methodsWithErrors = ReflectionUtils.getUniqueDeclaredMethods(testClass, autowiredTestOrLifecycleMethodFilter);
			return methodsWithErrors.length == 0 ? "NO AUTOWIRED VIOLATIONS DETECTED" : String.format("Test methods and test lifecycle methods must not be annotated with @Autowired. You should instead annotate individual method parameters with @Autowired, @Qualifier, or @Value. Offending methods in test class %s: %s", testClass.getName(), Arrays.toString(methodsWithErrors));
		}, String.class);
		if (errorMessage != "NO AUTOWIRED VIOLATIONS DETECTED") {
			throw new IllegalStateException(errorMessage);
		}
	}
    
	// 생략
}

public class TestContextManager {
	private static final Log logger = LogFactory.getLog(TestContextManager.class);
	private final TestContext testContext;
	private final ThreadLocal<TestContext> testContextHolder;
	private final List<TestExecutionListener> testExecutionListeners;
	
	// 생략

	public void prepareTestInstance(Object testInstance) throws Exception {
		if (logger.isTraceEnabled()) {
			logger.trace("prepareTestInstance(): instance [" + testInstance + "]");
		}

		this.getTestContext().updateState(testInstance, (Method)null, (Throwable)null);
		Iterator var2 = this.getTestExecutionListeners().iterator();

		while(var2.hasNext()) {
			TestExecutionListener testExecutionListener = (TestExecutionListener)var2.next();

			try {
				testExecutionListener.prepareTestInstance(this.getTestContext());
			} catch (Throwable var5) {
				Throwable ex = var5;
				if (logger.isErrorEnabled()) {
					logger.error("Caught exception while allowing TestExecutionListener [%s] to prepare test instance [%s]".formatted(typeName(testExecutionListener), testInstance), ex);
				}

				ReflectionUtils.rethrowException(ex);
			}
		}

	}

    // 생략
}
```

### validateAutowiredConfig() 메서드

- 테스트 메서드와 테스트 생명주기 메서드에 `@Autowired`가 잘못 사용되었는지 검증하는 메서드
- `AUTOWIRED_VALIDATION_NAMESPACE`에 대한 저장소를 가져옴
- `store.getOrComputeIfAbsent()`를 통해 테스트 클래스에 대한 검증을 수행
- `ReflectionUtils.getUniqueDeclaredMethods()`를 통해 테스트 클래스의 메서드 중 `autowiredTestOrLifecycleMethodFilter`에 해당하는 메서드를 가져옴
    - `autowiredTestOrLifecycleMethodFilter`: `@Autowired`를 직접 메서드에 붙인 메서드들만 필터링하는 조건

### TestContextManager 클래스

- ThreadLocal을 통해 스레드 별로 ApplicationContext를 생성한

### prepareTestInstance() 메서드

- `TestContextManager`가 테스트 인스턴스를 준비하는 메서드
- getTestContext()로 얻은 TestContext 객체의 상태를 테스트 인스턴스와 동기화
- `getTestExecutionListeners()` 메서드를 통해 등록된 `TestExecutionListener`들을 가져와 각각 반복
    - testContext에서 `@MockBean`, `@SpyBean` 등이 붙어있는 객체들에 Mock 객체 주입
