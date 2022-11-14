## 경쟁 상태 (Race Condition)

: 두 개 이상의 프로세스 또는 스레드들이 동시에 하나의 자원에 접근해 결과값에 영향을 줄 수 있는 상태

<br>

> ### Race Condition이 발생하는 경우

- #### 커널 모드에서 작업 수행 중 `Interrupt` 발생해 같은 데이터를 조작하는 경우
- #### 프로세스가 `System Call`을 하여 커널 모드로 진입해 작업을 수행하는 도중 `Context Switching`이 발생한 경우
- #### 멀티 프로세스 환경에서 공유 메모리 내의 `커널 데이터에 접근`한 경우

<br>

> ### Race Condition 해결을 위한 충족조건 3가지

- `상호 배제` (Mutual Exclusion)
  - 어떤 프로세스가 임계 영역에서 수행 중이면 다른 프로세스들은 접근 불가능
- `진행` (Progress)
  - 임계영역 내에 있는 프로세스 외에는 다른 프로세스의 임계영역 접근을 막아선 안됨
- `한정 대기` (Bounded Waiting)
  - 기아 상태(Starvation)을 방지하기 위해 임계 영역에 들어가려고 요청한 후부터 다른 프로세스들이 임계 영역에 들어가는 `횟수에 한계`가 있어야 함

<br>

---

### Reference

- [@gyoogle](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/Race%20Condition.md)
- [@zangzangs](https://zangzangs.tistory.com/115)
