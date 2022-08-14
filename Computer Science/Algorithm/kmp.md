## **KMP 알고리즘**

&nbsp; KMP(; Knuth-Morris-Pratt) 알고리즘은 문자열 중에 특정 패턴을 찾아내는 문자열 검색 알고리즘의 하나다. 이 알고리즘을 만든 Knuth, Morris, Pratt의 이름 중 첫 알파벳을 따 KMP 알고리즘이라고 부른다.

&nbsp; KMP 알고리즘은 `접두사`와 `접미사`를 활용하여 중복 연산을 줄인다. 접두사는 문자열에서 앞쪽을 떼어내어 만든 부분 문자열을 말하고, 접미사는 문자열 전체에서 뒷쪽을 떼어내어 만든 부분 문자열을 말한다. 이 알고리즘을 구현할 때 `table` 배열을 하나 선언해줘야 한다. 문자열의 0부터 i인 부분 문자열에서 길이가 `value`인 접두사와 길이가 `value`인 접미사가 일치한다고 했을 때, 이 table 배열의 인덱스 i에 해당하는 요소의 값은 value의 최대값이 된다. 단, 접두사와 접미사가 부분 문자열 전체와 일치해서는 안된다.

|  i  |  부분문자열  | table[i] |
| :-: | :----------: | :------: |
|  0  |      A       |    0     |
|  1  |      AB      |    0     |
|  2  |   `A`B`A`    |    1     |
|  3  |     ABAC     |    0     |
|  4  |  `A`BAC`A`   |    1     |
|  5  |  `A`BACA`A`  |    1     |
|  6  | `AB`ACA`AB`  |    2     |
|  7  | `ABA`CA`ABA` |    3     |

&nbsp; 위 표는 `ABACAABA`의 table 배열이다. 부분 문자열의 접두사와 접미사가 일치하는 최댓값을 각 요소에 저장해주었다. 아래 코드는 table 배열을 생성해주는 메서드다. (`자바 코드`)

```java
public static void makeTable(String pattern) {
	int len = pattern.length();
	table = new int[len];

	int index = 0;
	for(int i = 1; i < len; i++) {
		while(index > 0 && pattern.charAt(i) != pattern.charAt(index)) {
			index = table[index - 1];
		}

		if(pattern.charAt(i) == pattern.charAt(index)) {
			index += 1;
	    	table[i] = index;
		}
	}
}
```

&nbsp; 이렇게 table 배열을 선언 및 초기화 후 문자열을 비교한다. `makeTable()` 메서드를 사용해 만든 table 배열을 통해 두 문자열을 비슷한 방식으로 비교하여 문자열 `str`에 문자열 `pattern`이 포함되는지 탐색할 수 있다.

```java
public static int search(String str, String pattern) {
    int sLen = str.length();
    int pLen = pattern.length();

    int index = 0;
    for(int i = 0; i < sLen; i++) {
    	while(index > 0 && str.charAt(i) != pattern.charAt(index)) {
    		index = table[index - 1];
    	}

    	if(str.charAt(i) == pattern.charAt(index)) {
    		if(index == pLen - 1) {
    			index = table[index];
    			return 1;
    		}
    		else {
    			index++;
    		}
    	}
    }
	return 0;
}
```

</br>

### **관련 문제**

- [BOJ 16916 부분문자열](https://www.acmicpc.net/problem/16916)
- [BOJ 1786 찾기](https://www.acmicpc.net/problem/1786)

</br>

---

### **참고자료**

- Web
  - [#wikipedia](https://en.wikipedia.org/wiki/Knuth–Morris–Pratt_algorithm)
  - [@hanyeop](https://hanyeop.tistory.com/355?category=942306)
  - [@junstar92](https://junstar92.tistory.com/112?category=884881)
  - [@wns7756](https://m.blog.naver.com/wns7756/221852410609)
