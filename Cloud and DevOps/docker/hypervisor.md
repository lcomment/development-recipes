## 가상화와 하이퍼 바이저

> ### 1. 가상화 등장 이전

- 어떤 서비스를 운영하기 위해 애플리케이션이 OS 단위로 올라가 있음
- OS들은 하드웨어에 종속된 환경으로 구성됨
- 간단히 말하면, 1PC 1Application

<br>

> ### 2. 가상화(Virtualization)란?

: 가상화를 관리하는 소프트웨어를 사용하여 하나의 물리적 머신에서 가상 머신(VM)을 만드는 프로세스

- 애플리케이션이 OS 위가 아닌 VM 위에 올라감
- 하나의 PC에 많은 애플리케이션 가동 가능
- 하이퍼바이저가 필요에 따라 각 VM에 컴퓨팅 리소스를 할당

<br>

> ### 3. 하이퍼바이저 (Hypervisor)

: 가상 머신(Virtual Machine, VM)을 생성하고 구동하는 소프트웨어

- 하이퍼바이저 운영 체제와 가상 머신의 리소스를 분리해 VM의 생성과 관리를 지원
  - 하이퍼바이저로 사용되는 물리 하드웨어 → `호스트`
  - 리소스를 사용하는 여러 VM → `게스트`
- CPU, 메모리, 스토리지 등의 리소스를 처리하는 풀
  - 기존 게스트 간 또는 새로운 가상 머신에 쉽게 재배치 가능
- 서로 다른 OS들을 나란히 구동 가능 및 동일한 가상화 하드웨어 리소스를 공유

<br>

> ### 4. Linux는 하이퍼바이저를 사용하지 않는다?

- 리눅스는 하이퍼바이저를 가지고 있지 않음
- 리눅스 커널 → 직접 하드웨어를 관리하고 여러 프로세스를 실행하는 운영 체제
  - 컨테이너 기반의 가상화를 위한 기능을 내장
  - 리눅스 커널의 `Namespace`와 `Cgroups` 활용
    - Docker → 컨테이너 격리 및 리소스를 관리하여 가상화를 구현

<br>

> ### References

- [https://www.redhat.com/ko/topics/virtualization/what-is-a-hypervisor](https://www.redhat.com/ko/topics/virtualization/what-is-a-hypervisor)
- [https://mangkyu.tistory.com/86](https://mangkyu.tistory.com/86)
