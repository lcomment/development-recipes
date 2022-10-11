## CCW 알고리즘

&nbsp; CCW 알고리즘은 `Counter Clockwise`의 약자로, 평면 위에 놓여진 세 점의 방향관계를 구할 수 있는 알고리즘이다. 이 알고리즘은 점 A, B, C를 순서대로 봤을 때, 반시계 방향으로 놓여 있으면 양수를, 시계방향이면 음수를, 평행하게 놓여있으면 0을 반환한다.

```java
int ccw(Point A, Point B, Point C) {
		int a = A.x*B.y + B.x*C.y + C.x*A.y;
		int b = A.y*B.x + B.y*C.x + C.y*A.x;

		return a-b;
	}
```

&nbsp; 위의 코드는 CCW 알고리즘을 Java 코드로 구현한 것이다. CCW 알고리즘은 기하 알고리즘의 기초라고 할 수 있으니 알아두도록 하자.
