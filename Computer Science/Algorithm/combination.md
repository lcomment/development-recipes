## 조합 (Combination)

&nbsp; 조합(Combination)이란 n개 중 r개를 중복과 순서 없이 뽑는 경우의 수를 말한다. 순서가 없다는 점에서 순열(Permutation)과 다르다.

&nbsp; 알고리즘 문제를 풀다보면 문제의 서브 알고리즘으로 조합을 사용하는 경우가 종종 있다. 만약 `C++`을 사용한다면 algorithm 라이브러리의 `prev_permutation()` 함수를 이용하여 조합을 간단하게 구현해주면 되고, `python`을 사용한다면 그보다 더 쉽게 `from itertools import combinations`로 조합을 import 해준 후 `combinations()` 함수를 사용하면 된다. 그렇다면 `Java`는? Java는 조합 관련 라이브러리가 존재하지 않는다. 따라서 메서드로 구현해주어야 한다.

## 구현

```java
// combination(arr, visited, 0, n, r, 0)

static int[] combination(int[] arr, boolean[] visited, [] int start, int n, int r, int cnt) {
    if(cnt == r){
        return save(arr, visited, r)
    }

    for(int i=start ; i<n ; i++){
        visited[i] = true;
        combination(arr, visited, i+1, n, r, cnt+1);
        visited[i] = false;
    }
}

static int[] save(int[] arr, boolean[] visited, int r) {
    int[] c = new int[r];
    int idx = 0;

    for(int i=0 ; i<visited.length ; i++) {
        if(visited[i]){
            c[idx++] = arr[i];
        }
    }
    return c;
}
```

&nbsp; 만약 2차원 배열에서 조합을 사용한다면 2차원 배열을 1차원 배열로 나열하고 아래와 같이 `/` 연산자와 `%` 연산자를 이용하여 row와 column을 구분해주면 된다.

```java
static int[] combination(int[][] arr, boolean[][] visited, [] int start, int n, int r, int cnt) {
    if(cnt == r){
        return save(arr, visited, r)
    }

    for(int i=start ; i<n ; i++){
        int x = i / arr[0].length;
        int y = i / arr[0].length;

        if(!visited[x][y]){
            visited[x][y] = true;
            combination(arr, visited, i+1, n, r, cnt+1);
            visited[x][y] = false;
        }

    }
}
```

</br>

---

### **참고자료**

- Web
  - [@bcp0109](https://bcp0109.tistory.com/15)
