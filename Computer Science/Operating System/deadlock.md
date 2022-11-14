## 교착 상태 (DeadLock)

: 한전된 자원을 여러 프로세스가 사용하고자 할 때 발생하는 상황

- 자원 A를 점유한 `P1`과 자원 B를 점유한 `P2`
- P1은 B를, P2는 A를 필요로 함
- DeadLock 발생
- 프로세스가 자원을 얻기 위해 무한 대기

<br>

> ### 교착상태의 4가지 조건

- `상호배제` (Mutual Exclusion)
  - Only one process may use a resource at a time.
  - `오직 하나의 프로세스`만이 자원을 사용할 수 있다.
- `점유대기` (Hold and Wait)
  - A process may hold allocated resources while awaiting assignment of others
  - `이미 자원을 점유`하고 있는 프로세스가 다른 프로세스가 점유한 자원을 `요청`한다.
- `비선점` (No Preemption)
  - No resource can be forcibly removed from a process holding it.
  - 자원을 보유중인 프로세스에서 `강제`로 자원을 뺏어갈 수 없다.
- `환형 대기` (Circular Wait)
  - A closed chain of processes exists, such that each process holds at least one resource needed by the next process in the chain.
  - 상대 프로세스가 가진 자원에 대해 `순환상태로 서로 대기`한다.

<br>

> ### 교착상태 예방

- 상호배제 부정
  - 여러 프로세스가 `공유 자원` 사용
  - 자원들은 근본적으로 공유가 불가능
- 점유대기 부정
  - 프로세스 실행 전 `모든 자원`을 할당
  - 자원을 전혀 갖고 있지 않을 때만 자원요청 허용
  - `기아상태`(Starvation)이 발생할 수 있음
- 비선점 부정
  - 자원을 점유 중인 프로세스가 다른 자원을 요구할 때 가진 `자원 반납`
  - 이미 실행한 작업의 상태를 잃지 않도록 주의해야 함
- 환형대기 부정
  - 자원에 고유번호 할당 후 오름차순으로만 자원 요구

<br>

> ### 교착상태 회피

- 프로세스 시작 중단
  - 각 프로세스마다 요청과 해제에서 `정확한 순서를 파악`하고, 요청에 따른 프로세스 `대기 여부를 결정`
- 자원 할당 거부
  - `은행가 알고리즘` 이용
  - 프로세스가 자원을 요청할 때, `자원 할당 후에도 안정상태인지` 시스템이 사전에 검사

<br>

> ### 회복

- 교착 상태가 제거될 때까지 교착상태의 `프로세스 종료`
- 교착 상태의 프로세스가 점유하고 있는 자원을 `선점`해 다른 프로세스에게 할당

<br>

---

### 참고자료

- [@gold-dragon](https://gold-dragon.tistory.com/263)
- [@momoiot](https://momoiot.co.kr/iot-tech/scheduler/)
- [@hoyeonkim795](https://hoyeonkim795.github.io/posts/교착상태해결방법/)
