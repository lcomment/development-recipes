## Github

> ### 깃허브란?

: 분산 버전 컨트롤 SW 깃(Git)을 기반으로 소스 코드를 `호스팅`하고, `협업` 지원 기능들을 지원하는 Microsoft의 웹 서비스

<br>

> ### 깃허브의 플랜과 요금

→ [공식 요금 페이지](https://github.com/pricing)

- 개인용 플랜

  - Free
    - public repository 사용
    - private repository 사용 (협업자 수의 제한 및 일부 기능 제약)
    - Github Actions 시간 → 2,000분/월
    - Package Storage 용량 → 500MB
  - Pro
    - public repository 사용
    - private repository 사용
    - Github Actions 시간 → 3,000분/월
    - Package Storage 용량 → 1GB

- 팀용 플랜

  - Team
    - `Organization`을 만들고, 이 아래에서 repository를 관리하는 방식
    - 공개 저장소에 한해, Organization 기능 역시 기본적으로 무료 플랜으로 제공
    - Organization 아래에 다수의 팀을 만들고, 관리할 수 있는 기능과 추가적인 협업 기능 제공
  - Enterprise
    - 다수의 Organization을 일괄적으로 관리해야하는 경우 사용
    - 깃허브의 `셀프 호스티드` 버전
    - `로컬 머신`이나 `클라우드`에 직접 설치해서 사용
    - 감사 로그, 싱글 사이온, LDAP 인증과 같은 기능을 추가적으로 제공

- 무료 지원
  - 오픈소스와 교육, 비영리 단체에 대한 지원을 하고 있음

<br>

> ### 깃허브의 서비스들

- **Repository**
  - Git 원격 저장소 호스팅
  - 로컬 개발 환경과 온라인에서 안전하게 깃 저장소 접근 가능
  - 호스팅뿐 아니라, `이슈 트랙커`, `풀리퀘스트`, `소스코드 탐색`, `위키`, `인사이트` 등의 기능을 제공
  - 무료 플랜 → Private Repository에서 위키, 인사이트, 깃허브 페이지 기능 사용 불가능

<br>

- **Github Pages**
  - 깃허브 저장소를 기반으로 정적 파일들을 `호스팅`할 수 있는 서비스
  - HTML, CSS, JS로 구성된 파일을 올려두고 웹사이트 공개 가능
  - 내부적으로 `ruby`로 만들어진 정적 사이트 생성기 `지킬(Jekyll)` 빌드 지원
  - Private Repository를 호스팅하기 위해서는 프로 플랜을 사용해야 함

<br>

- **Github Actions**
  - 깃허브 깃 저장소에 통합되어 제공되는 `지속적 통합 서비스(CI)`
  - `action`이라는 단위로 작업 수행 → `workflow`
  - Marketplace에서 이미 만들어져 있는 것을 사용하거나 JS 또는 도커를 사용해 직접 작성

<br>

- **Github Package**

  - 깃허브 저장소와 통합해 언어별로 패키지를 `저장`하고 `호스팅` 할 수 있는 서비스
  - 지원 패키지
    - Node js → `npm`
    - Ruby → `gem`
    - Java → `mvn`, `gradle`
    - Docker → `Container Image`
    - .NET → `NuGet`
  - Private Repository의 경우, Storage와 데이터 사용량에 따라 별도의 요금제 적용

<br>

- **Github Container Registry**
  - 깃허브 패키지의 도커 이미지 저장 기능은 깃허브 저장소에 `종속`돼있음
  - 깃허브 컨테이너 레지스트리를 사용하면 저장소와 무관하게 계정이나 Organization 단위로 이미지를 `푸시`하거나 `권한 관리` 가능

<br>

- **GitHub Marketplace**
  - 깃허브와 연동해서 사용할 수 있도록 개발된 `써드파티`의 앱이나 액션을 구매할 수 있는 서비스

<br>

- **GitHub Sponsors**

  - 깃허브 사용자나 팀을 `정기 후원`할 수 있는 서비스

<br>

- **Gist**
  - 짧은 코드 조각을 공유할 수 있는 서비스

<br>

---

### 참고자료

- [@44bits](https://www.44bits.io/ko/keyword/github#저장소repository)
