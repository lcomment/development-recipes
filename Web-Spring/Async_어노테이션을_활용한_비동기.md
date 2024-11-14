# @Async를 활용한 비동기 처리

## @Async

- Spring에서 제공하는 Thread Pool을 활용하는 비동기 메소드 지원 Annotation
- Configuration 클래스에 `@EnableAsync`를 추가하여 사용 가능

### 기존 Java로 작성된 비동기 로직

```java
public class Test {

    private static ExecutorService executorService = Executors.newFixedThreadPool(5);

    public void runAsync(final Runnable runnable) {
        executorService.submit(runnable);
    }
}
```

### @Async를 활용한 비동기 로직

```java
public class Test {

    @Async
    public void asyncMethod(final Runnable runnable) {
        runnable.run();
    }
}
```

## @EnableAsync

### 1. @EnableAsync과 AsyncConfigurationSelector

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import({AsyncConfigurationSelector.class})
public @interface EnableAsync {
    Class<? extends Annotation> annotation() default Annotation.class;

    boolean proxyTargetClass() default false;

    AdviceMode mode() default AdviceMode.PROXY;

    int order() default Integer.MAX_VALUE;
}

public class AsyncConfigurationSelector extends AdviceModeImportSelector<EnableAsync> {
	// 생략

	@NonNull
	public String[] selectImports(AdviceMode adviceMode) {
		String[] var10000;
		switch (adviceMode) {
			case PROXY -> var10000 = new String[]{ProxyAsyncConfiguration.class.getName()};
			case ASPECTJ -> var10000 = new String[]{"org.springframework.scheduling.aspectj.AspectJAsyncConfiguration"};
			default -> throw new IncompatibleClassChangeError();
		}

		return var10000;
	}
}
```

- @Async의 default 모드는 `AdviceMode.PROXY`

### 2. ProxyAsyncConfiguration

```java
@Configuration(
    proxyBeanMethods = false
)
@Role(2)
public class ProxyAsyncConfiguration extends AbstractAsyncConfiguration {
    // 생략

    @Bean(
        name = {"org.springframework.context.annotation.internalAsyncAnnotationProcessor"}
    )
    @Role(2)
    public AsyncAnnotationBeanPostProcessor asyncAdvisor() {
        Assert.state(this.enableAsync != null, "@EnableAsync annotation metadata was not injected");
        AsyncAnnotationBeanPostProcessor bpp = new AsyncAnnotationBeanPostProcessor();
        bpp.configure(this.executor, this.exceptionHandler);
        Class<? extends Annotation> customAsyncAnnotation = this.enableAsync.getClass("annotation");
        if (customAsyncAnnotation != AnnotationUtils.getDefaultValue(EnableAsync.class, "annotation")) {
            bpp.setAsyncAnnotationType(customAsyncAnnotation);
        }

        bpp.setProxyTargetClass(this.enableAsync.getBoolean("proxyTargetClass"));
        bpp.setOrder((Integer)this.enableAsync.getNumber("order"));
        return bpp;
    }
}
```

- `AsyncAnnotationBeanPostProcessor`으로 등록

### 3. AsyncAnnotationBeanPostProcessor

```java
public class AsyncAnnotationBeanPostProcessor extends AbstractBeanFactoryAwareAdvisingPostProcessor {
	// 생략

	public void setBeanFactory(BeanFactory beanFactory) {
		super.setBeanFactory(beanFactory);
		AsyncAnnotationAdvisor advisor = new AsyncAnnotationAdvisor(this.executor, this.exceptionHandler);
		if (this.asyncAnnotationType != null) {
			advisor.setAsyncAnnotationType(this.asyncAnnotationType);
		}

		advisor.setBeanFactory(beanFactory);
		this.advisor = advisor;
	}
}
```

- @Async 애너테이션이 사용된 메서드를 프록시 객체로 변환
- 해당 메서드가 호출될 때 비동기 실행이 가능하도록 설정
- advisor로 AsyncAnnotationAdvisor 세팅

### 4. AsyncAnnotationAdvisor

```java
public class AsyncAnnotationAdvisor extends AbstractPointcutAdvisor implements BeanFactoryAware {
	// 생략
	
	protected Advice buildAdvice(@Nullable Supplier<Executor> executor, @Nullable Supplier<AsyncUncaughtExceptionHandler> exceptionHandler) {
		AnnotationAsyncExecutionInterceptor interceptor = new AnnotationAsyncExecutionInterceptor((Executor)null);
		interceptor.configure(executor, exceptionHandler);
		return interceptor;
	}
	
	// 생략
}
```

- AnnotationAsyncExecutionInterceptor 생성

### 5. AnnotationAsyncExecutionInterceptor이 상속한 AsyncExecutionInterceptor

```java
public class AsyncExecutionInterceptor extends AsyncExecutionAspectSupport implements MethodInterceptor, Ordered {
    // 생략

	@Nullable
	public Object invoke(final MethodInvocation invocation) throws Throwable {
		Class<?> targetClass = invocation.getThis() != null ? AopUtils.getTargetClass(invocation.getThis()) : null;
		Method specificMethod = ClassUtils.getMostSpecificMethod(invocation.getMethod(), targetClass);
		Method userDeclaredMethod = BridgeMethodResolver.findBridgedMethod(specificMethod);
		AsyncTaskExecutor executor = this.determineAsyncExecutor(userDeclaredMethod);
		if (executor == null) {
			throw new IllegalStateException("No executor specified and no default executor set on AsyncExecutionInterceptor either");
		} else {
			Callable<Object> task = () -> {
				try {
					Object result = invocation.proceed();
					if (result instanceof Future<?> future) {
						return future.get();
					}
				} catch (ExecutionException var5) {
					ExecutionException ex = var5;
					this.handleError(ex.getCause(), userDeclaredMethod, invocation.getArguments());
				} catch (Throwable var6) {
					Throwable exx = var6;
					this.handleError(exx, userDeclaredMethod, invocation.getArguments());
				}

				return null;
			};
			return this.doSubmit(task, executor, invocation.getMethod().getReturnType());
		}
	}
	
    // 생략
}
```

- invoke() 메서드를 통해 비동기 처리를 수행

### 6. AbstractAdvisingBeanPostProcessor에서 프록시 생성

```java
public abstract class AbstractAdvisingBeanPostProcessor extends ProxyProcessorSupport implements SmartInstantiationAwareBeanPostProcessor {
    // 생략
    
	public Object postProcessAfterInitialization(Object bean, String beanName) {
		if (this.advisor != null && !(bean instanceof AopInfrastructureBean)) {
			if (bean instanceof Advised) {
				Advised advised = (Advised)bean;
				if (!advised.isFrozen() && this.isEligible(AopUtils.getTargetClass(bean))) {
					if (this.beforeExistingAdvisors) {
						advised.addAdvisor(0, this.advisor);
					} else {
						if (advised.getTargetSource() == AdvisedSupport.EMPTY_TARGET_SOURCE && advised.getAdvisorCount() > 0) {
							advised.addAdvisor(advised.getAdvisorCount() - 1, this.advisor);
							return bean;
						}

						advised.addAdvisor(this.advisor);
					}

					return bean;
				}
			}

			if (this.isEligible(bean, beanName)) {
				ProxyFactory proxyFactory = this.prepareProxyFactory(bean, beanName);
				if (!proxyFactory.isProxyTargetClass()) {
					this.evaluateProxyInterfaces(bean.getClass(), proxyFactory);
				}

				proxyFactory.addAdvisor(this.advisor);
				this.customizeProxyFactory(proxyFactory);
				ClassLoader classLoader = this.getProxyClassLoader();
				if (classLoader instanceof SmartClassLoader) {
					SmartClassLoader smartClassLoader = (SmartClassLoader)classLoader;
					if (classLoader != bean.getClass().getClassLoader()) {
						classLoader = smartClassLoader.getOriginalClassLoader();
					}
				}

				return proxyFactory.getProxy(classLoader);
			} else {
				return bean;
			}
		} else {
			return bean;
		}
	}
	
	// 생략
}
```

- isEligible()가 true일 경우 ProxyFactory에 의해 Proxy 생성
