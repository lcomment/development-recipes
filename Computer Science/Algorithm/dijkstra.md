## **다익스트라 알고리즘** (Dijkstra's Algorithm)

: 그래프에서 한 정점에서 다른 정점까지의 최단 경로를 구하는 알고리즘 중 하나

- 도착 정점뿐 아니라 모든 정점까지의 최단 경로를 모두 찾음
- 매번 최단 경로의 정점을 선택해 탐색 반복
- 음의 가중치가 존재하지 않는다는 가정 안에서 그리디 알고리즘의 관점으로 동작
- 구현 방법이 여러 가지고, 어떤 방법을 쓰느냐에 따라 시간복잡도가 달라짐

</br>

### **동작 순서**

<p align=center><img src='../../resources/algorithm/dijkstra.png'></p>

- ① 최단 비용 테이블 초기화
- ② 출발 정점 선택
- ③ Loop:
  - 현재 정점 방문 처리
  - 현재 정점 기준 인접 노드 최소 비용 계산
  - 현재 정점의 방문하지 않은 인접 노드 중 거리가 가장 짧은 노드를 선택
- ④ 모든 정점 방문 시, Loop를 빠져나가면서 종료

</br>

## **구현**

### **DIJKSTRA(G, w, s)**

```
// Initialize
for each vertex v ∈ G.V
    v.d = ∞
    v.π = NIL

S = Ø    // 공집합
Q = G.V  // 전체집합

while Q != Ø
    u = EXTRACT-MIN(Q)
    S = S U {u}

    for each vertex v ∈ G.adj[u]
        // Relax
        if v.d > u.d + w(u, v)
            v.d = u.d + w(u, v)
            v.π = u
```

### **i) 순차 탐색 이용**

- 각 정점마다 인접한 간선들을 모두 검사
- 시간복잡도: (V-1) x V = O(V²)
  - V: 노드의 수

</br>

### **ii) 우선순위 큐 이용**

- 우선순위 큐에 원소를 넣고 삭제
- 시간복잡도: O(E•logV)
  - E: 간선의 수
  - V: 노드의 수

</br>

---

### **참고자료**

- Web
  - [@gomgomkim](https://gomgomkim.tistory.com/19)
