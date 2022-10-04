## 순열 (Permutation)

&nbsp; 순열(Permutation)이란 n개 중 r개를 중복 없이 뽑아 `정렬`하는 경우의 수를 말한다. 정렬, 즉 `순서가 있다`는 점에서 조합(Combination)과 다르다.

&nbsp; 알고리즘 문제를 풀다보면 문제의 서브 알고리즘으로 순열을 사용하는 경우가 종종 있다. 만약 `C++`을 사용한다면 algorithm 라이브러리의 `prev_permutation()` 함수를 이용하면 되고, `python`을 사용한다면 그보다 더 쉽게 `from itertools import permutations`로 순열을 import 해준 후 `permutations()` 함수를 사용하면 된다. 그렇다면 `Java`는? Java는 순열 관련 라이브러리가 존재하지 않기 때문에 메서드로 구현해주어야 한다. 방법이 여러가지 있지만, 다음 예제는 `백트래킹`으로 구현한 것이다.

```java
void permutation(int[] arr, int[] out, boolean[] visited, int depth, int r){
    if(depth == r){
        for(int num: out)
            System.out.print(num);
        System.out.println();
        return;
    }

    for(int i=0; i<arr.length; i++){
        if(!visited[i]){
            visited[i] = true;
            out[depth] = arr[i];
            permutation(arr, out, visited, depth+1, r);
            visited[i] = false;
        }
    }
}
```

<br>

## 중복순열 (Permutation With Repetition)

&nbsp; 중복순열은 n개 중 `중복 가능`하게 r개를 뽑아 `정렬`하는 경우의 수를 말한다. 순열과 비슷하게 구현 가능하고, 가장 큰 차이점은 방문을 체크하는 `visited 배열의 유무`다.

```java
void permutation(int[] arr, int[] out, int depth, int r){
    if(depth == r){
        for(int num: out)
            System.out.print(num);
        System.out.println();
        return;
    }

    for(int i=0; i<arr.length; i++){
        out[depth] = arr[i];
        permutation(arr, out, depth+1, r);
    }
}
```

<br>

---

### **참고자료**

- Web
  - [@cgw0519](https://velog.io/@cgw0519/알고리즘-순열-중복순열-조합-중복조합-총정리)
