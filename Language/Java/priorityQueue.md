## PriorityQueue

&nbsp; 먼저 들어오는 데이터가 먼저 나가는 FIFO(First-In-First-Out) 형식의 자료구조인 큐(Queue)와 다르게 **우선순위 큐(Priority Queue)** 는 `우선순위`가 높은 데이터가 먼저 나가는 형태의 자료구조이다. 일상생활에서는 병원의 응급실을 대표적인 예로 들 수 있겠다.

&nbsp; 우선순위 큐는 `힙(heap)`을 이용하여 구현하는 것이 일반적이다. 따라서 데이터를 삽입할 때, 우선순위를 기준으로 `최대 힙(max heap)` 혹은 `최소 힙(min heap)`을 구성한다. 또 데이터를 꺼낼 때는 루트 노드를 얻어내고, 루트 노드를 삭제할 때는 빈 루트 노드 위치에 맨 마지막 노드를 삽입한 후 아래로 내려가면서 적절한 자리를 찾아서 옮기는 방식으로 진행된다.

> **힙(heap)** : 완전 이진 트리의 일종으로 우선순위 큐를 위하여 만들어진 자료구조

</br>

## 우선순위 큐의 특징

- 높은 우선순위의 요소를 먼저 꺼내 처리하는 구조
- 내부 요소는 힙으로 구성돼 이진트리 구조로 이루어져 있다.
- 시간 복잡도는 O(nlogn)이다.
- 우선순위를 중요시해야 하는 상황에서 주로 쓰인다.

</br>

## Java에서 PriorityQueue 클래스 사용하기

&nbsp; 자바에는 우선순위 큐가 이미 라이브러리에 구현돼 있다. (따라서 직접 구현하는 것은 나중에 해보겠다.) 그럼 이제부터 자바에서 PriorityQueue 클래스를 사용해보자.

> ### Step1. Import

```java
import java.util.PriorityQueue;
import java.util.Collections;
```

&nbsp; PriorityQueue 객체를 생성하기 전에 먼저 import 과정을 거쳐야 한다. 추가로 최대 힙까지 알아보기 위해 Collections까지 import 해주자.

> ### Step2. Priority Queue 선언

```java
PriorityQueue<Integer> priorityQueue_Min = new PriorityQueue<>();
PriorityQueue<Integer> priorityQueue_Max = new PriorityQueue<>(Collections.reverseOrder());
```

&nbsp; 다른 객체를 생성하는 것과 똑같이 생성해주면 된다. 만약 최대 힙으로 구성된 PriorityQueue 객체를 생성하고 싶다면 `Collections.reverseOrder( )`을 이용해주면 된다.

> ### Step3. 삽입

```
priorityQueue_Min.add(1);
priorityQueue_Min.add(2);
priorityQueue_Min.offer(3);

priorityQueue_Max.add(1);
priorityQueue_Max.add(2);
priorityQueue_Max.offer(3);
```

&nbsp; 삽입을 할 때는 add( )나 offer( )를 사용한다.

&nbsp; 둘의 차이점이 무엇인지 의문을 가질 수도 있는데, `add( )`는 `Collection`에서 나오고, `offer( )`는 `Queue`에서 나온다. 용량 제한이 있을 시 add( )는 `항상 true를 반환`하므로, 요소를 추가할 수 없는 경우 `예외를 throw`한다. 반면, offer( )는 요소를 추가할 수 없는 경우 `false를 반환`한다. 하지만 PriorityQueue는 힙에 근거하는 무제한의 우선순위 큐이므로 크게 신경쓰지 않아도 된다.

&nbsp; 그럼 삽입되는 과정을 참고 그림을 통해 이해 해보자.

<p><img src="https://user-images.githubusercontent.com/56003992/129849467-a651893f-d020-4df2-bee8-1443bbd2337e.png" width="60%" height="60%"></p>

> ### Step4. 삭제

```java
// 첫번째 값을 반환하고 제거, 비어있다면 null
priorityQueue_Min.poll();

// 첫번째 값 제거, 비어있다면 예외 발생
priorityQueue_Min.remove();

// 초기화
priorityQueue_Min.clear();
```

&nbsp; 삭제할 때는 poll( ), remove( )을 사용한다. 반환을 하고 삭제할 것인지, 아닌지에 대해 판단하여 사용하면 된다. 추가로 초기화 시에는 clear( )을 사용한다.

> ### Step5. 우선순위(priority)가 가장 높은 값 출력

```java
PriorityQueue<Integer> priorityQueue_Min = new PriorityQueue<>();// 최소 힙
priorityQueue_Min.offer(2);     // priorityQueue에 값 2 추가
priorityQueue_Min.offer(1);     // priorityQueue에 값 1 추가
priorityQueue_Min.offer(3);     // priorityQueue에 값 3 추가
priorityQueue_Min.peek();       // priorityQueue에 첫번째 값 참조 = 1
priorityQueue_Min.element();	// // priorityQueue에 첫번째 값 참조 = 1

PriorityQueue<Integer> priorityQueue_Max = new PriorityQueue<>(Collections.reverseOrder());// 최대 힙
priorityQueue_Max.offer(2);     // priorityQueue에 값 2 추가
priorityQueue_Max.offer(1);     // priorityQueue에 값 1 추가
priorityQueue_Max.offer(3);     // priorityQueue에 값 3 추가
priorityQueue_Max.peek();       // priorityQueue에 첫번째 값 참조 = 3
priorityQueue_Max.element();	// // priorityQueue에 첫번째 값 참조 = 3
```

&nbsp; 앞에서 본 코드이기 때문에 peek( )와 element( )를 제외하고는 생소하지 않을 것이다. peek( )는 첫 번째 값을 반환하고 제거하지는 않는데, 만약 큐가 비어있다면 null을 반환한다. element( )도 peek( )와 같지만, 큐가 비어있다면 예외를 발생시킨다. 이 두 메서드를 이용하여 우선순위가 가장 높은 값을 출력하면 된다.
