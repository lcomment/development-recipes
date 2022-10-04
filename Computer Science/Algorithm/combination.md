## 조합 (Combination)

&nbsp; 조합(Combination)이란 n개 중 r개를 중복과 순서 없이 뽑는 경우의 수를 말한다. 순서가 없다는 점에서 순열(Permutation)과 다르다.

&nbsp; 순열과 마찬가지로 조합 또한 많이 쓰이는 알고리즘이다. 만약 `C++`을 사용한다면 algorithm 라이브러리의 `prev_permutation()` 함수를 이용하여 조합을 간단하게 구현해주면 되고, `python`을 사용한다면 그보다 더 쉽게 `from itertools import combinations`로 조합을 import 해준 후 `combinations()` 함수를 사용하면 된다. `Java`는 순열과 마찬가지로 존재하지 않아 구현해줘야 한다. 방법이 여러가지 있지만, 다음 예제는 `백트래킹`으로 구현한 것이다.

<br>

## 구현

```java
// combination(arr, visited, 0, n, r, 0)

int[] combination(int[] arr, boolean[] visited, [] int start, int n, int r, int cnt) {
    if(cnt == r){
        return save(arr, visited, r)
    }

    for(int i=start ; i<n ; i++){
        visited[i] = true;
        combination(arr, visited, i+1, n, r, cnt+1);
        visited[i] = false;
    }
}

int[] save(int[] arr, boolean[] visited, int r) {
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

<br>

## 중복조합 (Combination With Repetition)

&nbsp; 중복조합은 n개 중 `순서 없이`, `중복 가능`하게 r개를 뽑는 경우의 수를 말한다. 조합과 비슷하게 구현 가능하고, 가장 큰 차이점은 방문을 체크하는 `visited 배열의 유무`다.

```java
public static void combinationRepetition(int[] arr, int[] out, int start, int depth, int r){
    if(depth == r){
        for(int num : out)
            System.out.print(num);
        System.out.println();
        return;
    }
    for(int i=start; i<arr.length; i++){
        out[depth] = arr[i];
        combination(arr, out, i, depth+1, r);
    }
}
```

</br>

---

### **참고자료**

- Web
  - [@bcp0109](https://bcp0109.tistory.com/15)
  - [@cgw0519](https://velog.io/@cgw0519/알고리즘-순열-중복순열-조합-중복조합-총정리)
