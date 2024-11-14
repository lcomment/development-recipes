# GC 튜닝하기

## GC 튜닝에 앞서

- 튜닝이 필요 없다고 판단 되는 경우
  - `-Xms` 옵션과 `-Xmx` 옵션으로 메모리 크기를 지정함
  - `-server` 옵션이 포함돼 있음
  - 시스템에 `Timeout` 같은 로그가 없음
- GC 튜닝은 가장 마지막에 하는 것
  - 불필요한 객체 생성을 먼저 줄여라 (ex. String)
- GC 튜닝의 목적
  - Old 영역으로 넘어가는 객체 수를 최소화 하자
  - Major GC 실행 시간을 줄이자

## Old 영역으로 넘어가는 객체 수를 최소화 하자

- G1 GC를 제외한 대부분의 GC는 Generational GC
- Old 영역으로 가는 객체 수를 줄이면 `Major GC`의 수를 줄일 수 있음
- `Young 영역`의 크기를 잘 조절하면 큰 효과를 볼 수 있음

## Major GC 시간 줄이기

- Major GC는 Minor GC보다 10배 정도 긺
- 실행 시간이 길어지면 여러 부분에서 Timeout이 날 수 있음
- 그렇다고 Old 영역을 줄이면 `OOME`(Out Of Memory Error)가 발생하거나 Major GC 횟수가 늘어감
- 반대로 늘리면 Major GC 횟수는 줄지만, 실행시간이 늘어남
- 따라서 `Old 영역` 크기를 적절하게 설정해야 함

## GC 성능을 결정하는 옵션

- 서비스마다 객체의 크기와 라이프 사이클이 다르므로 다른 사례를 모방하면 안됨
- 옵션을 많이 설정한다고 빨라지지 않음
- 두 대 이상의 서버에 GC 옵션을 다르게 적용하고, `성능이나 GC 시간이 개선`된 때에만 적용하자

### Heap 영역 크기

- -Xms
  - JVM 시작 시 힙 영역 크기
- -Xmx
  - 최대 힙 영역 크기

### Young 영역 크기

- -XX:NewRatio
  - Young 영역과 Old 영역의 비율
- -XX:NewSize
  - New 영역의 크기
- -XX:SurvivorRatio
  - Eden 영여과 Survivor 영역의 비율

### GC 방식

- -XX:+UseSerialGC
  - Serial GC
- -XX:+UseParallelGC
  - Parallel GC
- -XX:+UseConcMarkSweepGC
  - CMS GC
- -XX:+UseG1GC
  - G1 GC
- -XX:+UseZGC
  - ZGC

---

### Reference

- [@NaverD2](https://d2.naver.com/helloworld/37111)