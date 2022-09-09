## REST란?

: `Representational State Transfer`의 약자로, 자원을 이름으로 구분하여 해당 상태를 주고받는 모든 것을 의미

- HTTP URI를 통해 `자원을 명시`
- HTTP Method를 통해 해당 자원에 대한 `CRUD Operation 적용`

### 구성 요소

- 자원(resources): HTTP URI
- 행위(verb): HTTP Method
- 내용(representations): HTTP Message Payload

### 특징

- Uniform Interface (유니폼 인터페이스)
  - URI로 지정한 리소스에 대한 조작을 `통일되고 한정적인 인터페이스`로 수행하는 아키텍처 스타일
- Stateless (무상태성)
  - 작업을 위한 `상태 정보`(세션, 쿠키 등)를 저장 및 관리하지 않음
  - API 서버는 클라이언트의 `요청`만 단순히 처리
- Cacheable (캐시 가능)
  - `HTTP`라는 웹 표준을 그대로 사용
  - 따라서 캐싱 기능 적용 가능
  - HTTP 프로토콜 표준에서 사용하는 `Last-Modified Tag`나 `E-Tag` 이용
- Server-Client Structure (서버-클라이언트 구조)
  - client와 server의 역할이 확실히 구분됨
  - 서로 간 의존성 감소
- Layered System (계층형 구조)
  - 다중 계층으로 구성될 수 있음
  - 보안, 로드밸런싱, 암호화 계층을 추가해 `구조상 유연성`을 둘 수 있음
  - 프록시, 게이트웨이 같은 `네트워크 기반의 중간매체`를 사용할 수 있게 함
- Self-descriptiveness (자체 표현 구조)
  - REST API 메시지만 보고도 쉽게 이해 가능

<br>

## REST API 디자인

- URI는 동사보다는 `명사`를, 대문자보다는 `소문자`를 사용하여야 한다.
- 계층 관계를 나타내는 슬래시(`/`)를 마지막에 사용하지 않는다.
- 언더바(`_`)보단 하이폰(`-`)을 사용한다.
- 파일확장자는 URI에 포함하지 않는다.
- 행위를 포함하지 않는다.

---

### **참고자료**

- Web
  - [@khj93](https://khj93.tistory.com/entry/네트워크-REST-API란-REST-RESTful이란)
  - [@meetup](https://meetup.toast.com/posts/92)
