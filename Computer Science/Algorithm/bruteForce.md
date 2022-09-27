## 1. Brute-Force Algorithm

→ 가능한 `모든` 경우의 수를 `모두` 체크해서 정답을 찾는 방법  
</br>

### 기본적인 2가지 규칙

- ① 사용된 알고리즘이 적절한가? (문제 해결이 가능한가?)
- ② 효율적으로 동작하는가?

&nbsp; 다른 알고리즘을 사용하면 O(N)으로 해결되는 어떤 문제가 있다고 가정했을 때, 이 문제를 O(N²)이 걸리는 Brute-Force를 사용하는 것은 옳지 않다! 따라서 아래와 같은 사항을 고려해서 수행해야 한다.

- 해결하고자 하는 문제의 가능한 경우의 수를 대략적으로 계산
- 가능한 모든 방법을 다 고려
- 실제 답을 구할 수 있는지 적용

<br>

## 2. 완전 탐색 기법

> ### i) 단순 완전탐색 문제

- 특별한 어느 기법을 사용하지 않고, 단순히 `반복문`과 `조건문` 등으로 모든 경우의 수를 만들어 답을 구하는 방식
- 시간복잡도가 O(N²)이나 `O(N!)`이기 때문에 케이스의 범위를 보고 사용해야 함
- 사실 간단한 문제이기 때문에 코딩 테스트 문제로 자주 출제되지 않음

<br>

> ### ii) 비트 마스크 (Bit-Mask)

- 비트 연산을 통해 부분 집합을 표현하는 방법
- and(`&`), or(`|`), not(`~`), xor(`^`), shift(`<<`, `>>`)

|  A  |  B  | A & B | A &#124; B | ~A  | A &#94; B |
| :-: | :-: | :---: | :--------: | :-: | :-------: |
|  0  |  0  |   0   |     0      |  1  |     0     |
|  0  |  1  |   0   |     1      |  1  |     1     |
|  1  |  0  |   0   |     1      |  0  |     1     |
|  1  |  1  |   1   |     1      |  0  |     0     |

<br>

> ### iii) Recursive

→ 말 그대로 자기 자신을 호출

- 재귀를 탈출하기 위한 `탈출 조건` 필요
- 현재 함수의 `상태를 저장`하는 Parameter 필요
- `return문`을 신경쓸 것

#### → DP와의 차이점

- DP: 작은 문제가 큰 문제와 `동일한 구조`를 가져 큰 문제의 답을 구할 시에 작은 문제의 결과를 `기억`한 뒤 그대로 사용하여 수행속도르 빠르게 함
- 완전탐색은 크고 작은 문제의 구조가 `다를 수 있고`, 해결 가능한 방법을 `모두 탐색`

<br>

> ### iv) 순열과 조합 (Permutation and Combination)

- [순열 (Permutation)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/permutaion.md)
- [조합 (Combination)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/combination.md)

<br>

> ### v) DFS / BFS

- [깊이 우선 탐색 (DFS)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/dfs.md)
- [너비 우선 탐색 (BFS)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/bfs.md)
