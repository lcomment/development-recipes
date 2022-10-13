## 로드 밸런싱 (Load Balancing)

: 네트워크 트래픽을 하나 이상의 서버나 장비로 분산하기 위해 사용되는 기술

<div align=center>
    <img src='../../resources/network/loadBalancing.png' width=400>
    <h5>출처: https://tecoble.techcourse.co.kr/post/2021-11-07-load-balancing/</h5>
</div>

### ✓ 트래픽 증가에 대한 처리 방식

- `Scale-Up` : CPU, 메모리, 디스크 등의 `기능 업그레이드`
  - 비용이 기하급수적으로 증가
  - 하나의 서버에서 웹 서비스 제공 → 서버 중지 및 장애 발생 시 가용성에 문제 발생
- `Scale-Out` : 저렴한 노드 여러 개를 하나의 `Cluster`로 구성하는 방식
  - 하나의 노드에 문제가 발생해도 웹 서비스 중단 X
  - Scale-Out 방식의 웹 서비스 구성에 로드 밸런싱을 주로 사용

<br>

## 로드 밸런싱의 방식

> ### ✓ Round Robin

- Real 서버로의 Session 연결을 순차적으로 맺어주는 방식
- 각 서버가 동일한 스펙을 가지고 있고, Session이 오래 지속되지 않는 경우 적합
- Session에 대해 보장을 제공하지 않음

> ### ✓ Weighted Round Robin

- 각 서버마다 `가중치`를 매기고 가중치가 높은 서버에 우선적으로 배정하는 방식
- 각 서버의 트래픽 처리 속도가 상이한 경우에 적합

> ### ✓ Hash

- Hash 알고리즘을 이용한 로드 밸런싱 방식
- Client가 해시함수를 통해 특정 Server로 연결된 이후 `동일 서버로만 연결`되는 구조
- `Session에 대한 보장` 제공

> ### ✓ Least Connection

- Session 수를 고려하여 `가장 작은 Session`을 보유한 서버로 Session을 맺어주는 연결 방식
- 서버에 분배된 트래픽이 일정하지 않은 경우 적합
- Session에 대한 보장을 제공하지 않음

> ### ✓ Response Time

- 응답시간을 고려하여 `빠른 응답시간`을 제공하는 서버로 Session을 맺어주는 방식
- Session에 대한 보장을 제공하지 않음
