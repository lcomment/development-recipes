## **Lombok에 대하여**

&nbsp; Java 기반의 개발을 할 때 우리는 Getter, Setter 등의 메서드를 만들어줘야 하고, 심지어는 toString() 메서드를 오버라이딩 하여 재구현해야 하는 귀찮음이 있다. 이런 귀찮음을 해결해주는 해결사가 바로 Lombok이다.

> ### **Lombok**: 어노테이션 기반으로 코드를 자동완성 해주는 라이브러리

<br>

### Lombok의 장점

- 어노테이션 기반의 코드 자동 생성을 통한 생산성 향상
- 코드 가독성 및 유지보수성 향상
- Getter, Setter 외에 Builder 패턴이나 Log 생성 등 다양한 방면으로 활용 가능

<br>

### Lombok 사용 시 주의할 점

&nbsp; 객체 간 상호 참조관계가 있을 때, Lombok이 toString() 메서드 자동 생성 시 무한 루프에 빠질 수 있다. 이 경우에는 옵션을 통해 참조하는 객체를 제외해줘야 한다.

<br>

### Lombok 설정

**→ Gradle 프로젝트**

```gradle
. . .

configuration {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

. . .

dependencies {
    . . .

    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'

    testComplileOnly 'org.projectlombok:lombok'
    testAnnotationProcessor 'org.projectlombok:lombok'

    . . .
}

. . .
```

**→ Maven 프로젝트**

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
    <scope>provided</scope>
</dependency>
```

## Lombok 활용

### @Getter와 @Setter

```java
@Getter
public class Test {
    private int id;
    @Setter
    private String name;
}
```

&nbsp; @Getter 어노테이션과 @Setter 어노테이션을 사용하여 손쉽게 Getter, Setter를 생성할 수 있다. 클래스 이름 위에 적용하면 모든 변수에 적용되고, 변수 이름 위에 적용하면 특정 변수만 적용된다. 위 코드에서는 id, name의 `Getter`와 name의 `Setter`가 생성된다.

<br>

### @AllArgsConstructor와 @NoArgsConstructor

```java
@AllArgsConstructor  // 또는 @NoArgsConstructor
public class Test {
    private int id;
    private String name;
}
```

&nbsp; 생성자를 자동완성 해주는 어노테이션이다. 이름에서 알 수 있듯이 `@AllArgsConstructor`는 모든 변수를 사용하는 생성자를, `@NoArgsConstructor`는 변수가 없는 기본 생성자를 생성해준다.

<br>

### @EqualsAndHashCode

```java
@AllArgsConstructor
@EqualsAndHashCode(of={"id", "name"}, callSuper=false)
public class Test  extends Parents {
    private int id;
    private String name;
}
```

&nbsp; `equals()` 메서드와 `hashCode()` 메서드를 생성해준다. `of` 옵션을 통해 구별해주는데, 위와 같이 of가 id와 name로 설정됐다면 id와 name이 동일할 시 같은 객체로 인식해준다. 또 `callSuper` 옵션은 상위 클래스의 경우 적용시킬지 설정한다. 위의 코드에서는 false이기에 상위 클래스의 경우 적용시키지 않는다.

<br>

### @ToString

```java
@ToString
public class Test  extends Parents {
    @ToString.Exclude
    private int id;
    private String name;
}
```

&nbsp; `toString()` 메서드를 클래스의 변수들을 기반으로 생성해준다. 만약 출력에서 제외하고 싶은 변수가 있다면 `@ToString.Exclude` 어노테이션을 붙여주면 된다. 상위클래스에 대한 적용은 `callSuper 옵션`을 사용하여 적용할 수 있다.

<br>

### @Data

&nbsp; @Data 어노테이션을 활용하면 @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor를 자동완성 시켜준다.

<br>

### @Builder

&nbsp; @Builder 어노테이션을 활용하면 해당 클래스의 객체의 생성에 Builder 패턴을 적용시켜준다. 만약 `모든 변수`들에 대해 Build를 원한다면 `클래스` 이름 위에, `특정 변수`만을 Builder하길 원한다면 생성자 작성 후 `생성자` 위에 @Builder 어노테이션을 붙여주면 된다.

```java
@NoArgsConstructor
@Getter
public class Test {
    private int id;
    private String name;
    private String nick;

    @Builder
    public Member(int id, String nick){
        this.id = id;
        this.nick = nick;
    }
}
```

&nbsp; 위와 같이 (name을 제외하고) id와 nick만 포함된 생성자 위에 @Builder 어노테이션을 적용하면 id와 nick만을 Build한다. 아래 코드와 같이 빌더를 사용할 수 있다.

```java
public class TestController {
    public ResponseEntity init() {
        Test test = Test.builder()
            .id("1")
            .nick("test")
            .build();

        return ResponseEntity.ok(test);
    }
}
```

---

### **참고자료**

- Web
  - [@phantom](https://phantom.tistory.com/63)
