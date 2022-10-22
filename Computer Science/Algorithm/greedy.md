# Greedy Algorithm

</br>
  
## 1. 그리디 알고리즘이란?

: 매 선택에서 현재 당장 최적해를 찾아 적합한 결과를 도출하자는 알고리즘 기법

- 현재 조건에서 `최선의 선택`을 하면서 최적해를 찾는다.
- 백트래킹을 통해 추가 점검을 하지 않는다.

<br>

> ### But! 그리디 알고리즘은 전체에서 최적해를 언제나 구하는 것은 아니다.

<div align="center">
  <img src="../../resources/algorithm/greedy1.png" width='400'>
</div>
  
&nbsp; 위의 그림을 보고 이해해보자. A에서 E로 가는 최적 경로를 구하고자 한다. 그리디 알고리즘의 정의에 따라 A에서 B, C, D 중 하나로 먼저 이동하게 되는데, 이 때 최적 경로는 **1**의 시간이 걸리는 C로 가는 것이다. 그러나 C를 거쳐 E로 가면 `151의 시간`이 걸리게 된다. 실제 최적 경로는 A에서 B를 거쳐 E로 가는 것이고, 이 때는 `11의 시간`이 걸린다. 최초의 선택에서 최적의 선택을 하여 부분 최적해는 구했지만, 결론적으로 최적해를 구하지 못하게 된 것이다.

&nbsp; 그렇다면 왜 그리디 알고리즘을 사용할까? 그리디 알고리즘은 속도가 매우 빠르기 때문이다. 하지만 위에서 말했듯 항상 최적해가 되지는 않기 때문에 특수한 조건을 걸어 이 조건을 만족해야만 사용할 수 있다.

<br>

## 2. 그리디 알고리즘을 사용할 수 있는 조건

> ### i) 탐욕 선택 속성 (Greedy Choice Property)
>
> : 이전의 선택이 이후 영향을 주지 않음
>
> ### ii) 최적 부분 구조 (Optimal Substructure)
>
> : 부분 문제의 최적결과가 전체에도 그대로 적용됨

&nbsp; ii번 조건을 보고 DP와 같다고 생각할 수 있지만, DP는 이전의 경우에 영향을 받기 때문에 그리디 알고리즘과는 다르다. 아무튼 위의 조건을 만족할 때 그리디 알고리즘은 다른 경우의 결과와 상관 없이 최적해를 구할 수 있게 된다.

&nbsp; 하지만 저 두 조건을 완전히 만족하는지 판단하기는 생각보다 어렵고, 그렇기 때문에 일반적으로 증명을 위해 수학적 증명이 동반되는데, 이 또한 쉽지 않기 때문에 테스트 코드를 작성하여 알아보는 방식을 사용하는 경우가 많다.

<br>

## 3. 예시

> ### Coin Change Problem 1
>
> &nbsp; A가 3,270원 어치의 물건을 사고 5,000원을 냈다고 가정하면 카운터 직원은 1,730원의 거스름돈을 A에게 줘야 한다. 이 때, 지폐와 동전의 단위가 다음과 같다면 최소 개수로 거스름돈을 주는 경우는?  
> → 지폐 및 동전의 단위: 1,000원, 100원, 50원, 10원

**_sol)_** 간단한 문제기 때문에 해결할 방법은 여러가지겠지만, 일반적으로 큰 단위의 화폐부터 계산하는 `MSD`(Most Significant Digit) 방식을 사용할 것이다. 따라서 이전의 선택과 관련 없고, 그리디 알고리즘을 사용할 수 있다.

**_ans)_** 1,000원: 1개, 100원: 7개, 10원: 3개

```java
public class Solution{
  public int solution(int charge, int[] money) {
    int n = 0;

    for(int i : money){
      if(charge % i == 0){
        int d = charge / i;
        n += d;
        charge -= i*d;
      }
      if(charge == 0) return n;
    } // for_i
  }
}
```

- 예시문제
  - [BOJ 11047 동전 0](https://www.acmicpc.net/problem/11047)

</br>

> ### Coin Change Problem 2
>
> B는 H-마트에서 물건을 샀고, 카운터 직원은 120원의 거스름돈을 돌려줘야 하는 상황일 때, 동전의 단위가 다음과 같다면 최소 개수로 거스름돈을 주는 경우는?  
> → 지폐 및 동전의 단위: 50원, 40월, 10원

**_sol)_** Quiz 01과 같은 방식으로 구해보면 50원 2개, 10원 2개로 총 4개의 동전으로 거슬러 줄 수 있다. 하지만 실제로는 40원짜리 동전 3개를 거슬러 주는 것이 최적해다. 40원짜리 동전은 보다 큰 가치의 동전(50원)의 약수가 아니기 때문이다. 따라서 50원은 40원을 대체할 수 없게 된다. 따라서 조건을 만족하지 못했기에 그리디 알고리즘을 사용할 수 없고, 백트래킹을 활용한 `완전탐색(Brute-Force Search)`를 이용하여 해결해야 한다.

<br>

> ### Activity-Selection Problem (활동 선택 문제)
>
> &nbsp; N개의 활동이 있고 각 활동에는 시작 시간(s) 및 종료 시간(f)이 있을 때, 한 사람이 할 수 있는 최대 활동의 수를 구하는 문제

<div align="center">
  <img src="../../resources/algorithm/greedy2.png" width='500'>
</div>

</br>
  
**_sol)_**  
1️⃣ 종료 시간 기준으로 `정렬` 후 맨 앞 요소인 (i=1) select  
2️⃣ (i=1)의 종료시간 이후 가능 활동인 (i=4) select  
3️⃣ (i=4)의 종료시간 이후 가능 활동인 (i=8) select  
4️⃣ (i=8)의 종료시간 이후 가능 활동인 (i=11) select  
5️⃣
</br>

이제 코드로 나타내보자. 먼저 Action 클래스가 다음과 같이 구성돼 있다고 가정하자.

```java
class Action {
    String name;
    int s;
    int f;

    public Action(String name, int startTime, int finishTime){
        this.name = name;
        this.s = startTime;
        this.f = finishTime;
    }
}
```

</br>    
  
 다음은 Solution 코드이다.
``` java
public class Solution{
  public ArrayList<Action> solution(ArrayList<Action> actionList) {
    // 종료시간 기준 정렬 (같을 시 시작시간 정렬)
    Arrays.sort(actionList, new Comparator<Action>() {
      @Override
      public int compare(Action a, Action b){
        int result = a.f - b.f;
        
        if(result == 0)
          return a.s-b.s;
        return result;
      } // compare
    });
    
    int time = 0;
    ArrayList<Action> doList = new ArrayList<>();
    
    for(Action act : actionList){
      if(time <= act.s){
        time = act.f;
        doList.add(act);
      }
    }
    
    return doList;
  } // method
} // class
```

- 예시 문제
  - [BOJ 1931 회의실 배정](https://www.acmicpc.net/problem/1931)
  - [BOJ 11000 강의실 배정](https://www.acmicpc.net/problem/11000)
