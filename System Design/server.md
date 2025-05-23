# 서버와 로드밸런서

## I. 단일 서버

- 한 서버에 웹 서버, DB, 캐시 등을 모두 실행하는 구성
- 실제 운영 환경에서는 웹 서버와 DB를 각각 다른 서버에 구성
- 이후, 캐시 등 `시스템의 컴포넌트를 분리`하여 각각 독립적으로 확장할 수 있는 환경 구성

### 동작 과정

1. Client가 DNS로 도메인을 보냄
2. DNS가 받은 도메인을 IP 주소로 변환하여 Client에게 전달
3. Client가 해당 IP 주소로 HTTP 요청 전달
4. 요청 받은 웹 서버가 응답 반환

## II. Scale Up과 Scale Out

### Scale Up

- 기존 자원을 고사양 자원을 변경
- 매우 간단하지만, 심각한 단점을 가지고 있음
  - 규모 확장에 한계가 있음
  - 장애 시 `자동복구`(failover)나 `다중화`(redundancy)가 불가능

### Scale Out

- 서버나 컴퓨팅 리소스를 수평으로 확장하는 방법
- 여러 노드에 작업을 `분산`하여 시스템 전체의 처리 성능을 높임
- 필요에 따라 쉽게 서버를 `추가`하거나 `제거`하여 급격한 트래픽 증가에 유연하게 대응
- Scale Up보다 비용이 저렴

## III. 로드밸런서

- 부하 분산 집합에 속한 웹 서버들에게 트래픽 부하를 고르게 분산하는 역할을 함
  - 서버 1 다운 → 모든 트래픽을 서버 2에게 전송
  - 트래픽의 양에 따라 서버가 추가되거나 제거될 수 있음
- 사용자는 로드밸런서의 Public IP에 접속
  - 서버 간 통신에는 Private IP 이용
