## GC (Garbage Collection)


: 불필요한 메모리 낭비를 방지하기 위해 `JVM`이 불필요한 메모리를 정리해주는 기능

### 전체적인 흐름

- Java는 `Heap` 영역에 `참조 타입`(인스턴스나 동적 할당된 데이터)을 저장
- `Young Generation` 공간: 새로 생성된 인스턴스 저장
  - `Minor GC`가 일어남
- `Old Generation` 공간 : Minor GC를 여러 번 거친 인스턴스들을 저장
  - `Major GC`가 일어남

### GC가 기본으로 하는 JVM의 2가지 대전제

- 대부분의 객체는 쉽게 `Garbage`가 된다.
- Old generation 영역의 객체가 Young generation의 객체를 참조하는 일은 드물다.

## GC의 동작 방식

### 기본 프로세스
  - `Marking`
    - GC가 사용 중인 메모리와 사용하지 않는 메모리를 식별한다.
  - `Normal Deletion`
    - `참조되지 않는 객체`를 메모리에서 삭제
    - Memory allocator가 메모리에서 삭제된 여유 공간에 대한 포인터를 가지고 있음
    - 새로운 메모리 할당이 요구될 때를 대비
  - `Deletion with Compacting`
    - 성능을 향상 시키기 위해 참조되지 않는 `객체를 삭제한 뒤 남은 참조된 객체들`을 압축
    - 이렇게 하면 새로운 메모리 할당이 더 간단하고 빨라진다.

### 공통 동작 방식

- `Stop the world`
  - JVM이 GC를 실행하기 위해 애플리케이션의 실행을 멈추는 작업
  - GC를 실행하는 thread 외 다른 모든 `thread의 작업 중단`
- `Mark and Sweep`
  - GC가 스택의 모든 변수 or 접근 가능한 객체 `스캔`
  - `Mark`: 사용되지 않는 메모리 식별 과정
  - `Sweep`: 사용되지 않는 메모리 제거 과정

### Minor GC

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

### Major GC

1. Young generation 영역에서 인스턴스들이 넘어옴
2. Old Generation 영역의 메모리가 부족 → `Major GC 발생`

→ 크키가 작은 Young Generation에서의 Minor GC에 비해 Major GC는 `10배 이상의 시간` 소모 가능

## GC의 종류

### Serial GC (-XX:+UseSerialGC)

- 하나의 쓰레드로 GC 실행
- `mark-sweep-compact` 방식
- 모든 GC 작업이 애플리케이션 실행을 중단시키는 방식
- Stop The World 시간이 긺
- `싱글 쓰레드 환경` 및 Heap이 매우 작을 때 사용

### Parallel GC (-XX:+UseParallelGC)

- 여러 개의 쓰레드로 GC 실행
- Serial GC와 동일하게 `mark-sweep-compact` 방식
- `Young Generation` 영역에서 `Parallel GC`를 사용
- Java 8의 default GC 방식

### CMS GC (-XX:+UseConcMarkSweepGC)

- `Stop The World` 시간을 줄이기 위해 개발된 GC
- GC 작업을 애플리케이션과 동시에 실행
- 동작 방식
  - `Initial Mark`: 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체(`GC Root가 참조하는 객체`)만 찾음
  - `Concurrent Mark`: 방금 살아있다고 확인한 객체에서 `참조하고 있는 객체`들을 따라가면서 확인
  - `Remark`: `Concurrent Mark` 중 새로 `추가`되거나 `참조`가 끊긴 객체를 찾음
  - `Concurrent Sweep`: 사용하지 않는 객체를 삭제
- 모든 애플리케이션의 `응답 속도`가 매우 중요할 때 사용
- 단점
  - 다른 GC 방식보다 메모리와 CPU를 더 많이 사용함
  - Compaction 단계가 기본적으로 제공되지 않음
- Java 9부터 deprecated 되었고 결국 Java 14에서는 사용이 중지

### G1 GC (-XX:+UseG1GC)

- Garbage First (G1)
- CMS GC를 대체하기 위해 jdk 7 버전에서 최초로 release된 GC
- Java 9+ 버전의 디폴트 GC로 지정
- 4GB 이상의 힙 메모리, Stop the World 시간이 0.5초 정도 필요한 상황에 사용
  - Heap이 작을 경우, 미사용 권장
- Young 영역, Old 영역이 아닌 `Region`이라는 개념을 새로 도입하여 사용
  - 전체 Heap 영역을 Region으로 체스 같이 분할
  - 상황에 따라 Eden, Survivor, Old 등 역할을 고정이 아닌 `동적`으로 부여
- Garbage로 가득찬 영역을 빠르게 회수하여 빈 공간을 확보
  - 결국 `GC 빈도가 줄어드는 효과`를 얻게 되는 원리

### ZGC (-XX:+UseZGC)

- Java 15 버전에서 추가된 GC
- 대량의 메모리(8MB ~ 16TB)를 낮은 지연으로 처리하기 위해 설계된 GC
- Heap이 큰 애플리케이션에서도 성능을 유지
- Concurrent Mark-and-Relocate` 방식
- `ZPage`라는 영역 사용

### Reference

- [@whitepro](https://whitepro.tistory.com/462)
