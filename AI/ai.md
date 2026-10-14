# AI Recipe

&nbsp; LLM을 활용해 애플리케이션을 만드는 백엔드 개발자 관점의 레시피 입니다. 모델을 직접 학습시키는 내용보다, `이미 만들어진 모델을 어떻게 서비스에 녹여낼 것인가`에 초점을 맞췄습니다. 프롬프트 설계부터 토큰·비용 관리, RAG, 에이전트까지 실제 서비스에서 마주치는 주제를 다룹니다. 필요한 레시피를 편하게 얻어가시고, 원하는 레시피는 `ISSUE`에 등록, 추가하고 싶은 레시피와 잘못된 레시피는 `PR`에 등록해주시면 감사하겠습니다.

## Ch01. LLM 기초

- [LLM이란?](https://github.com/lcomment/development-recipes/blob/main/AI/LLM/llm_기초.md)
- 토큰과 컨텍스트 윈도우
- Function Calling과 도구 사용
- 구조화된 출력 (Structured Outputs)
- 프롬프트 캐싱

## Ch02. RAG (Retrieval-Augmented Generation)

- RAG란?
- 임베딩과 벡터 검색
- 벡터 데이터베이스
- 청킹(Chunking) 전략
- 검색 품질 개선 (Reranking, Hybrid Search)

## Ch03. AI 에이전트

- AI 에이전트란?
- 에이전트 루프와 도구 설계
- MCP (Model Context Protocol)
- 멀티 에이전트와 작업 분배

## Ch04. AI 엔지니어링 패러다임

- [프롬프트 엔지니어링 (Prompt Engineering)](https://github.com/lcomment/development-recipes/blob/main/AI/Paradigm/프롬프트_엔지니어링.md)
- 컨텍스트 엔지니어링 (Context Engineering)
- 에이전틱 엔지니어링 (Agentic Engineering)
- 하네스 엔지니어링 (Harness Engineering)
- 리버스 엔지니어링 (Reverse Engineering)

## Ch05. 실전 운영

- 평가(Eval) 설계와 LLM-as-judge
- 토큰 이코노미 (비용과 레이턴시 최적화)
- 옵저버빌리티 (트레이싱과 토큰 사용량 추적)
- 가드레일과 프롬프트 인젝션 방어
- 모델 마이그레이션

## Ch06. Claude Code

- Claude Code란?
- CLAUDE.md로 프로젝트 컨텍스트 주기
- 권한과 설정 (settings.json)
- 커스텀 슬래시 커맨드
- 스킬 (Agent Skills)
- 서브에이전트 활용
- 훅(Hooks)으로 워크플로우 자동화
- MCP 서버 연동
- 컨텍스트 관리와 플랜 모드
- CI/CD에서 헤드리스로 실행하기
