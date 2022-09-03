## Java의 람다식

&nbsp; 자바에 큰 변화를 준 것이 두 개가 있는데, 그 중 하나는 JDK 1.8부터 추가된 람다식이다. 람다식으로 인해 자바는 객체지향언어인 동시에 함수형 언어가 되었다. 덕분에 우리는 함수형 언어의 장점들을 자바에서도 누릴 수 있게 되었는데, 그럼 지금부터 람다식에 대해 알아보자.

&nbsp; **람다식**이란 메서드를 하나의 식으로 표현한 것으로, `익명함수`라고도 부른다. 바로 예제를 보면서 이해해보자.

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random() * 5 + 1);

int method(){
	return (int)(Math.random() * 5) + 1;
}
```

&nbsp; 위의 코드에서 `( ) -> (int)(Math.random()*5+1`이 람다식이다. 람다식은 `method( )`를 간결하게 표현한 것이다. 코드를 봤을 때 당연히 람다식이 더 이해하기 쉬울 것이다. 또 메서드로 표현하려면 클래스를 만들어야 하고, 호출하려면 객체까지 생성해야 한다. 하지만 람다식을 사용한다면 이 과정들을 거치지 않아도 된다. 또 람다식은 `메서드의 매개변수`로도 전달 가능하고, `메서드의 결과`로 반환될 수도 있다.

## 람다식 표현

&nbsp; 람다식은 메서드에서 이름과 반환타입을 제거하고, 매개변수 선언부와 몸통{ } 사이에 `->`를 추가해주면 된다. 반환값이 있는 메서드의 경우, return문 대신 식으로 대신 할 수 있는데, 이럴 경우 문장이 아닌 식이므로 세미콜론을 붙이지 않아도 된다. 예시를 보면서 이해해보자.

```java
// 1번
int max(int a, int b){
    return a > b ? a : b;
}

// 2번
(int a, int b) -> { return a > b ? a : b; }

// 3번
(int a, int b) -> a > b ? a : b

```

&nbsp; 람다식에 선언된 매개변수의 타입은 `추론이 가능한 경우 생략 가능`한데, 대부분의 경우 생략 가능하다. 람다식에 반환타입이 없는 이유 또한 항상 추론이 가능하기 때문이다. 위의 예제에서도 a와 b의 타입을 지워도 상관없다. 이 때 `둘 중 하나만 지우는 것은 허용되지 않으므로 주의해야 한다.`

```java
(a, b) -> a > b ? a : b
```

&nbsp; 만약 매개변수가 하나뿐인 경우에는 `괄호도 생락` 가능하다. 단, 매개변수의 타입이 있다면 생략 불가능하다.

```java
a -> a * a  // Ok
int a -> a * a  // Error
```

&nbsp; 마찬가지로, 중괄호 안에 문장이 하나라면 이 또한 생략 가능하다.

```java
// 1번
(String name, int i) -> {
    System.out.println(name+"="+i);
}

// 2번
(String name, int i) -> System.out.println(name + "=" + i)
```

## 함수형 인터페이스(Functional Interface)

&nbsp; 자바에서는 모든 메서드가 클래스에 포함돼야 한다. 람다식 또한 마찬가지이고, 익명 클래스의 객체와 동등하다. 아래 코드를 보면서 이해해보자.

```java
// 1번
(int a, int b) -> a > b ? a : b

// 2번
new Object( ) {
    int max(int a, int b){
        return  a > b ? a : b;
    }
}
```

&nbsp; 그렇다면 람다식으로 정의된 익명 객체의 메서드를 어떻게 호출할 수 있는 것일까? 인터페이스가 그 정답을 가지고 있다. 아래의 코드를 보자. MyFunction이라는 인터페이스를 f라는 클래스에서 구현했다. 메서드 max( )를 보면 람다식 `(int a, int b) -> a > b ? a : b`와 선언부가 일치한다. 따라서 우리는 익명 객체를 람다식으로 대체할 수 있는 것이다.

```java
interface MyFunction {
 	public abstract int max(int a, int b);
}

MyFunction f = new MyFunction() {
 	public int max(int a, int b){
        return a > b ? a : b;
    }
};

MyFunction l = (int a, int b) -> a > b ? a : b;
```

&nbsp; 실제로 람다식 또한 `익명 객체`이고, MyFunction 인터페이스를 구현한 익명 객체의 메서드 max( )와 매개변수의 타입, 개수, 그리고 리턴값이 일치하기 때문에 가능한 것이다. 이렇듯 인터페이스는 자바의 규칙들을 어기지 않았다. 따라서 인터페이스를 통해 람다식을 다루기로 하였고, 이 인터페이스를 `함수형 인터페이스`라고 부른다. 단, 함수형 인터페이스에는 오직 `하나의 추상 메서드만 정의`돼야 한다. 이를 통해 우리는 아래의 코드처럼 간단하게 코드를 짤 수 있게 되었다.

```java
List<String> list = Arrays.asList("abc", "aaa", "bbb", "ddd", "aaa");

// 기존 방식
Collections.sort(list, new Comparator<String>() {
	public int compare(String s1, String s2) {
    		return s2.compareTo(s1);
    }
});

// 람다식
Collections.sort(list, (s1, s2) -> s2.compareTo(s1));
```
