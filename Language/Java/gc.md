## Garbage Collection

: 불필요한 메모리 낭비를 방지하기 위해 `JVM`이 불필요한 메모리를 정리해주는 기능

> ### 전체적인 흐름

- Java는 `Heap` 영역에 `참조 타입`(인스턴스나 동적 할당된 데이터)을 저장
- `Young Generation` 공간: 새로 생성된 인스턴스 저장
  - `Minor GC`가 일어남
- `Old Generation` 공간 : Minor GC를 여러 번 거친 인스턴스들을 저장
  - `Major GC`가 일어남

<br>

> ### GC가 기본으로 하는 JVM의 2가지 대전제

- 대부분의 객체는 쉽게 `Garbag`e가 된다.
- Old generation 영역의 객체가 Young generation의 객체를 참조하는 일은 드물다.

<br>

## GC의 동작 방식

> ### 공통 동작 방식

- `Stop the world`
  - JVM이 GC를 실행하기 위해 애플리케이션의 실행을 멈추는 작업
  - GC를 실행하는 thread 외 다른 모든 `thread의 작업 중단`
- `Mark and Sweep`
  - GC가 스택의 모든 변수 or 접근 가능한 객체 `스캔`
  - `Mark`: 사용되지 않는 메모리 식별 과정
  - `Sweep`: 사용되지 않는 메모리 제거 과정

<br>

> ### Minor GC

→ Young generation 영역은 `Eden 영역`과 `Survivor 영역`으로 나뉨

- Eden: `새로 생성된 객체`가 할당되는 영역
- Survivor: 최소 1번 이상의 `GC 이후 살아남은` 객체가 존재하는 영역

→ 이 영역들이 Minor GC의 구성요소가 됨

1. 인스턴스 계속 생성 → `Eden 영역 포화`
2. Stop the world → Mark and Sweep
3. `2번 과정`에서 살아남은 객체가 `첫 번째 Survivor` 영역으로 이동
4. 첫 번째 Survivor 영역 포화 → GC 과정에서 살아남은 객체가 `두 번째 Survivor` 영역으로 이동
   - age: Survivor 영역에서 객체가 `살아남은 횟수` (Object Header에 `기록`)
5. 일정 Age 이상 살아남은 객체 → `Old generation` 영역으로 이동: `Promotion`

<br>

> ### Major GC

1. Young generation 영역에서 인스턴스들이 넘어옴
2. Old Generation 영역의 메모리가 부족 → `Major GC 발생`

→ 크키가 작은 Young Generation에서의 Minor GC에 비해 Major GC는 `10배 이상의 시간` 소모 가능

<br>

### Reference

- [@whitepro](https://whitepro.tistory.com/462)
