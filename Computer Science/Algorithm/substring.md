## **문자열 탐색**

&nbsp; 알고리즘 문제 중 가장 기본이라고 판단되는 유형은 문자열 문제다. "Hello World"라는 문자열에서 "Hello"를 찾는다고 가정하자. 우리가 간단하게 로직을 구현하여 쉽게 찾을 수 있다.

```java
// 자바 코드
public class SearchSubstring {
	public static void main(String[] args) throws IOException {
		String str = "Hello World";
        String find = "Hello";

        System.out.println(str.contains(find));
	}
}
```

&nbsp; 하지만 위와 같이 구현하거나 for문을 통한 완전탐색으로 구현하면, 입력을 받아 처리를 할 경우 최대 시간복잡도는 `O(N*M)`이다. 문자열의 길이가 길어질수록 걸리는 시간은 당연히 급격하게 증가해서 시간초과가 발생할 수 있다. 따라서 문자열 탐색에 유리한 알고리즘을 학습해야 할 필요가 있다.

<br>

## **문자열 탐색 알고리즘**

- ### [KMP 알고리즘](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/kmp.md)
- ### [Rabin-karp 알고리즘](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/rabinKarp.md)
