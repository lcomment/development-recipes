## Single-Source Shortest Path

&nbsp; 최단 경로 문제는 쉽게 접할 수 있는 알고리즘 문제 중 한 가지다. 최단 경로 문제에도 여러 유형이 있는데, 그 유형들은 다음과 같다.

- Single-Source (One to All)
- Single-Destination (All to One)
- Single-Pair (One to One)
- All-Pairs (All to All)

&nbsp; 이 네 가지 중 `Single-Source` 문제에 대해 다룰 예정이다. 이 문제에서 몇몇 특징을 찾을 수 있는데,

- 최단 경로는
  - `부분 경로` 또한 최단 경로이다.
  - `가중치(weight)`가 양이든, 음이든, 0이든 `사이클(Cycle)`을 포함하지 않는다.

<br>

- 그래프 𝐺(𝑉, 𝐸)의 경로는
  - 정점을 𝑉 이하 가진 것만 고려한다.
  - 간선은 𝑉-1개 이하인 것만 고려한다.

<br>

### Single-Source 문제 해결방법

- [Dijkstra 알고리즘](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/dijkstra.md)
- [Bellman-Ford 알고리즘](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/bellmanFord.md)
