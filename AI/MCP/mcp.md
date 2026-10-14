# MCP란?

- `MCP`(Model Context Protocol)는 AI 애플리케이션을 외부 시스템에 연결하기 위한 `개방형 표준`
- 공식 문서의 비유: `AI 애플리케이션을 위한 USB-C 포트`
  - USB-C가 기기 연결 방식을 표준화했듯, MCP는 AI 애플리케이션과 외부 시스템의 연결 방식을 표준화함
- 연결 대상은 크게 세 가지
  - `데이터`: 로컬 파일, 데이터베이스
  - `도구`: 검색 엔진, 계산기, 외부 API
  - `워크플로우`: 특화된 프롬프트 템플릿

> ⚠️ **작성 기준: 2026-07-28 사양**
> MCP는 사양 변화가 빠릅니다. 특히 `2026-07-28` 개정에서 세션이 제거되고 프로토콜이 상태 없음(stateless)으로 바뀌는 등 큰 변경이 있었습니다. 구버전 기준으로 쓰인 자료가 많으므로 사양 버전을 확인하고 읽어야 합니다.

<br>

## 왜 필요한가

### N × M 통합 문제

- MCP 이전에는 `AI 애플리케이션마다, 연동 대상마다` 통합 코드를 따로 만들어야 했음
  - 애플리케이션 N개 × 데이터 소스 M개 = N × M 개의 통합
- 표준이 생기면 `N + M`으로 줄어듦
  - 서버를 한 번 만들면 MCP를 지원하는 모든 클라이언트가 쓸 수 있음
  - 클라이언트를 한 번 만들면 모든 MCP 서버에 붙을 수 있음

### 이미 폭넓게 지원됨

- Claude, ChatGPT 같은 AI 어시스턴트
- VS Code, Cursor 같은 개발 도구
- 한 번 만들어 여러 곳에 붙이는 것이 실제로 가능한 수준의 생태계

<br>

## 아키텍처

### 참여자

| 참여자 | 역할 |
| --- | --- |
| `MCP Host` | AI 애플리케이션 본체. 여러 클라이언트를 조율·관리 |
| `MCP Client` | 서버 하나와 전용 연결을 유지하며 컨텍스트를 가져옴 |
| `MCP Server` | 컨텍스트를 제공하는 프로그램 |

- 호스트는 `연결할 서버마다 클라이언트를 하나씩` 생성함
  - 서버 3개에 붙으면 클라이언트 3개가 각각 전용 연결을 유지

```
┌─────────── MCP Host (예: VS Code) ───────────┐
│  Client 1      Client 2      Client 3        │
└─────┬──────────────┬──────────────┬──────────┘
      │              │              │
  Server A       Server B       Server C
 (파일시스템)    (데이터베이스)    (원격 API)
   로컬/stdio     로컬/stdio     원격/HTTP
```

- `서버`는 실행 위치와 무관한 개념임
  - 로컬에서 프로세스로 돌면 흔히 "로컬 서버", 원격에서 서비스로 돌면 "원격 서버"라 부르지만 둘 다 MCP 서버
  - 로컬 서버는 보통 클라이언트 하나를 상대하고, 원격 서버는 여러 클라이언트를 상대함

### 두 개의 레이어

| 레이어 | 담당 |
| --- | --- |
| `데이터 레이어` | JSON-RPC 2.0 기반 메시지 구조와 의미 — 무엇을 주고받는가 |
| `전송 레이어` | 연결 수립, 메시지 프레이밍, 인증 — 어떻게 주고받는가 |

- 데이터 레이어가 안쪽, 전송 레이어가 바깥쪽
- 전송 방식이 달라도 `JSON-RPC 메시지 형식은 동일`함

<br>

## Primitives — 서버가 제공하는 것

MCP의 핵심 개념이다. 서버는 세 가지를 노출할 수 있다.

| Primitive | 무엇인가 | 예시 |
| --- | --- | --- |
| `Tools` | 모델이 호출해 `동작을 수행`하는 함수 | 파일 쓰기, API 호출, DB 질의 |
| `Resources` | 모델에 `맥락을 제공`하는 데이터 | 파일 내용, DB 레코드, API 응답 |
| `Prompts` | 상호작용을 구조화하는 `재사용 템플릿` | 시스템 프롬프트, few-shot 예시 |

- 각 primitive에는 `*/list`(발견), `*/get`(조회), 그리고 Tools의 경우 `tools/call`(실행) 메서드가 있음
- 목록 조회가 별도 메서드인 덕에 `동적으로 바뀔 수 있음`
  - 서버 상태나 사용자 권한에 따라 노출되는 도구가 달라질 수 있음

> 구체적인 예: 데이터베이스를 다루는 MCP 서버라면 — 질의용 `Tool`, 스키마를 담은 `Resource`, 그 도구를 쓰는 few-shot 예시가 담긴 `Prompt`를 함께 노출할 수 있다.

### 클라이언트가 제공하는 것

- `Elicitation`: 서버가 사용자에게 `추가 입력이나 확인을 요청`하는 기능
  - 서버가 작업 중 사용자 확인이 필요할 때 사용

- 아래는 `2026-07-28 기준 폐기 예정(deprecated)`이므로 새로 구현할 때 채택하지 않는다

| 폐기된 기능 | 대체 방안 |
| --- | --- |
| `Sampling` (서버가 클라이언트의 모델에 추론 요청) | LLM 제공자 API를 직접 연동 |
| `Logging` (서버가 클라이언트에 로그 전송) | `stderr`로 출력하거나 OpenTelemetry 사용 |
| `Roots` (클라이언트가 작업 디렉토리 범위 전달) | 도구 파라미터, 리소스 URI, 서버 설정으로 전달 |

<br>

## 상태 없음(Stateless)과 discover

`2026-07-28` 사양의 가장 큰 변화다.

### 무엇이 바뀌었나

- `initialize` / `notifications/initialized` 핸드셰이크가 `제거됨`
- 프로토콜 레벨 세션과 `Mcp-Session-Id` 헤더가 `제거됨`
- 모든 요청이 `자기 자신을 처리하는 데 필요한 정보를 전부 담음`
  - 서버는 이전 요청으로부터 아무것도 추론하지 않음

### `_meta`로 매 요청에 담는 것

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "weather_current",
    "arguments": { "location": "Seoul" },
    "_meta": {
      "io.modelcontextprotocol/protocolVersion": "2026-07-28",
      "io.modelcontextprotocol/clientInfo": { "name": "example-client", "version": "1.0.0" },
      "io.modelcontextprotocol/clientCapabilities": { "elicitation": {} }
    }
  }
}
```

| 필드 | 용도 |
| --- | --- |
| `protocolVersion` | 이 요청에서 사용하는 프로토콜 버전 |
| `clientCapabilities` | 클라이언트가 처리할 수 있는 기능 |
| `clientInfo` | 클라이언트 식별 (설정으로 끌 수 있음) |

- 서버는 결과의 `_meta`에 `serverInfo`를 담아 자신을 식별함
- 버전이 맞지 않으면 `UnsupportedProtocolVersionError`로 지원 버전 목록을 돌려주고, 클라이언트가 재시도

### `server/discover`

- 서버가 `반드시 구현해야 하는` RPC
- 지원 프로토콜 버전, 기능(capabilities), 신원을 한 번에 알려줌
- 클라이언트는 다른 요청 전에 호출할 수 있지만 `필수는 아님`
  - 모든 요청이 `_meta`를 싣고 다니므로, 바로 요청을 보내고 버전 에러를 처리하는 것도 가능
- 응답은 `캐시 가능` — `ttlMs`(유효 시간)와 `cacheScope`(공유 가능 범위)를 함께 반환

### 상태가 필요하면

- 프로토콜이 상태를 들고 있지 않으므로, 서버가 `명시적인 핸들(handle)을 발급`해서 해결
  - 장바구니 ID, 워크플로우 ID 같은 것
  - 이 핸들을 `일반 도구 인자로` 매 요청에 함께 받음
- 중요: `핸들을 가지고 있다는 사실을 인증으로 취급하면 안 됨` (아래 보안 참고)

### 결과 타입

- 모든 결과에 `resultType` 필드가 필수
  - `"complete"`: 일반적인 완료
  - `"input_required"`: 추가 입력이 필요한 중간 결과
- 구버전 서버가 이 필드를 생략하면 클라이언트는 `"complete"`로 취급해야 함

### MRTR — 서버가 입력을 요청하는 방식

- 과거에는 서버가 클라이언트에 요청을 거는 방식(`sampling/createMessage` 등)이었으나, `Multi Round-Trip Requests` 패턴으로 대체됨

```
1. 클라이언트 → 서버 : 요청
2. 서버 → 클라이언트 : resultType "input_required" + inputRequests
3. 클라이언트 → 서버 : 같은 요청을 inputResponses와 함께 재시도
```

- 서버가 능동적으로 요청을 거는 대신, `클라이언트의 재시도`로 흐름이 이어짐

<br>

## 전송 방식

| | stdio | Streamable HTTP |
| --- | --- | --- |
| 통신 | 표준 입출력 스트림 | HTTP POST (+ 선택적 SSE 스트리밍) |
| 위치 | 같은 머신의 로컬 프로세스 | 원격 |
| 성능 | 네트워크 오버헤드 없음 | 네트워크 경유 |
| 인증 | 프로세스 경계가 곧 경계 | Bearer 토큰, API 키 등 — `OAuth 권장` |
| 상대 | 보통 클라이언트 1개 | 보통 클라이언트 다수 |

- `HTTP+SSE` 전송은 폐기됨. Streamable HTTP로 이전해야 함
- Streamable HTTP에서 `SSE 스트림 재개(resumability)가 제거됨`
  - 스트림이 끊기면 진행 중이던 요청은 유실됨
  - 클라이언트가 `새 요청 ID로 다시 보내야` 함

### 알림 받기

- 과거의 HTTP GET 엔드포인트와 `resources/subscribe`가 `subscriptions/listen` 하나로 통합됨
- `옵트인 방식` — 받고 싶은 알림 종류를 명시해야 함
  - `toolsListChanged`, `promptsListChanged`, `resourcesListChanged`, `resourceSubscriptions`
- 서버가 수락한 종류를 되돌려주고, 이후 알림마다 `subscriptionId`를 붙여 보냄
- `최선 노력(best effort)`일 뿐 보장되지 않음
  - 전송 재연결 과정에서 알림이 유실될 수 있으므로, 폴링으로도 신선도를 유지해야 함

<br>

## 캐싱

- `tools/list`, `prompts/list`, `resources/list` 등의 결과에 `ttlMs`와 `cacheScope`가 필수
  - `ttlMs`: 신선도 힌트. 이 시간 동안 캐시해도 된다는 의미
  - `cacheScope`: `public`(공유 중계자도 캐시 가능) 또는 `private`
- 서버는 `tools/list`를 `결정적 순서로` 반환해야 함
  - 클라이언트 측 캐싱을 가능하게 하고, `LLM 프롬프트 캐시 히트율`을 높이기 위함
  - 도구 목록의 순서가 매번 바뀌면 프롬프트 접두사가 깨져 캐시가 무효화됨 — [프롬프트 캐싱](https://github.com/lcomment/development-recipes/blob/main/AI/LLM/프롬프트_캐싱.md) 참고

<br>

## 보안 — 반드시 알아야 할 것

MCP 서버를 붙이는 것은 `외부 코드에 권한을 주는 일`이다. 공식 보안 문서가 다루는 주요 위협을 요약한다.

### 1. 토큰 패스스루 (금지됨)

- `MCP 서버는 자신에게 발급되지 않은 토큰을 받아들여서는 안 됨`
  - 클라이언트가 준 토큰을 검증 없이 하위 API로 그대로 넘기는 패턴
- 왜 위험한가
  - `audience` 검증을 건너뛰면 다른 서비스용 토큰이 통과함 — OAuth의 기본 보안 경계가 깨짐
  - 레이트 리밋, 요청 검증 같은 통제를 우회할 수 있음
  - 감사 로그에 실제 호출자가 남지 않아 사고 조사가 불가능해짐

### 2. 혼동된 대리자 (Confused Deputy)

- 서드파티 API를 중계하는 MCP 프록시 서버에서 발생
- `정적 client ID` + `동적 클라이언트 등록` + `동의 쿠키`가 겹칠 때, 공격자가 사용자 동의를 건너뛰고 인가 코드를 탈취할 수 있음
- 방어
  - 서드파티 인가 흐름으로 넘기기 `전에` 클라이언트별 동의를 받고 저장
  - `redirect_uri`를 등록값과 `정확히 일치`하는지 검증 (패턴·와일드카드 금지)
  - `state`를 서버 측에 저장하고 콜백에서 검증, 단일 사용 + 짧은 만료

### 3. 핸들 탈취 (State Handle Hijacking)

- 상태 없음 설계에서 쓰이는 서버 발급 핸들을 공격자가 획득·추측해 다른 사용자의 상태에 접근
- 방어
  - `핸들 소유를 인증으로 취급하지 않음` — 모든 요청을 별도로 인가 검증
  - 추측 불가능한 난수 핸들 사용, 만료 설정
  - 핸들을 `인증된 사용자에 서버 측에서 바인딩` (예: `<user_id>:<handle>`로 저장, 사용자 ID는 검증된 토큰에서 도출)

### 4. 로컬 서버 침해

- 로컬 MCP 서버는 사용자 머신에서 `클라이언트와 같은 권한으로` 실행되는 바이너리
- 설정에 악의적인 실행 명령을 심거나, 서버 자체에 악성 페이로드를 넣는 공격이 가능
- 방어
  - 원클릭 설정을 지원하는 클라이언트는 `실행될 명령 전문을 잘라내지 않고` 보여주고 명시적 승인을 받아야 함
  - 서버를 샌드박스에서 최소 권한으로 실행
  - 로컬 서버는 `stdio 전송`을 써서 접근 주체를 클라이언트로 제한

### 5. SSRF

- 악의적인 서버가 OAuth 메타데이터 발견 과정에서 `내부 주소를 가리키는 URL`을 돌려줄 수 있음
  - 클라우드 메타데이터 엔드포인트(`169.254.169.254`)로 자격증명 탈취
- 방어: HTTPS 강제, 사설·예약 IP 대역 차단, 리다이렉트 대상도 동일 검증, 이그레스 프록시 사용

### 6. 인가 URL 검증

- 서버가 준 인가 URL을 그대로 열면 XSS·RCE로 이어질 수 있음
- `http`/`https`만 허용하고 `javascript:`, `data:`, `file:` 등은 거부 (허용 목록 방식)
- URL을 열 때 `셸 명령을 쓰지 말 것`

### 7. 권한 범위 최소화

- `files:*`, `admin:*` 같은 넓은 스코프를 처음부터 받으면 토큰 탈취 시 피해 범위가 커짐
- 최소 스코프로 시작해, 권한이 필요한 동작을 처음 시도할 때 `단계적으로 상승`
- 안티패턴: 와일드카드 스코프, 미래를 대비한 권한 끼워넣기, 가능한 모든 스코프를 목록에 공개

### 그리고 — 도구 설명도 입력이다

- 서버가 주는 도구 설명과 응답은 모델이 읽는 `텍스트`임
  - 악의적인 서버가 설명이나 응답에 지시를 심으면 `프롬프트 인젝션` 경로가 됨
- 결국 `서버 측 인가 검사`가 유일하게 믿을 수 있는 방어선이라는 원칙은 여기서도 같음
  - [프롬프트 엔지니어링](https://github.com/lcomment/development-recipes/blob/main/AI/Paradigm/프롬프트_엔지니어링.md)의 프롬프트 인젝션 절 참고

<br>

## MCP를 쓸까, 직접 도구를 만들까

| 상황 | 선택 |
| --- | --- |
| 이미 공개된 MCP 서버가 있는 연동 대상 | `MCP` — 만들 필요 없음 |
| 여러 AI 클라이언트에서 같은 연동을 써야 함 | `MCP` — 한 번 만들어 재사용 |
| 팀·조직 내부 시스템을 여러 도구에 열어줘야 함 | `MCP` |
| 내 애플리케이션 한 곳에서만 쓰는 도구 | `직접 구현` — 프로토콜 오버헤드가 불필요 |
| 도구 호출 루프를 세밀하게 통제해야 함 | `직접 구현` |

- MCP는 `통합의 표준`이고, Function Calling은 `모델이 도구를 호출하는 메커니즘`
  - 둘은 대체 관계가 아님. MCP 서버의 Tool도 결국 모델에게는 호출 가능한 함수로 제시됨
- 도구 개수가 늘어나면 `도구 정의 자체가 입력 토큰`이라는 점을 잊지 말아야 함
  - 서버를 여러 개 붙이면 질문을 보내기 전부터 수천 토큰이 소모될 수 있음
  - [토큰과 컨텍스트 윈도우](https://github.com/lcomment/development-recipes/blob/main/AI/LLM/토큰과_컨텍스트_윈도우.md) 참고

<br>

## 참고

- [MCP 공식 문서](https://modelcontextprotocol.io)
- [Architecture overview (2026-07-28)](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture)
- [2026-07-28 사양 변경사항](https://modelcontextprotocol.io/specification/2026-07-28/changelog)
- [Security Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices)
- [MCP 참조 서버 구현](https://github.com/modelcontextprotocol/servers)
- 에이전트의 도구 사용 맥락은 [AI 에이전트란?](https://github.com/lcomment/development-recipes/blob/main/AI/Agent/ai_에이전트.md) 참고
