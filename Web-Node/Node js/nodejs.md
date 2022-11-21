## Node.js란?

: Chrome V8 Javascript 엔진으로 빌드된 `Javascript 런타임`

- 이벤트 기반의 Non-Blocking I/O 모델 → 가볍고 효율적
- 기본적으로 `Single-Thread` (Multi-Thread도 지원함)

<br>

## Event-Driven

: 이벤트가 발생할 때 미리 지정해둔 작업을 수행하는 방식

- 이벤트 리스너(Event Listener)에 콜백 함수를 등록해야 함
- 이벤트가 발생하면 미리 지정해둔 콜백 함수 호출

> ### 용어 정리

- 이벤트 루프 (Event Loop)
  - 이벤트 발생 시 호출할 콜백 함수들을 관리하고, 호출된 콜백 함수의 실행 순서 결정
- 백그라운드 (Background)
  - 타이머나 이벤트 리스너들이 대기하는 곳
- 테스크 큐 (Task Queue)
  - 이벤트 발생 후 호출돼야 할 콜백 함수들이 기다리는 공간

<br>

> ### 예제

```javascript
function run() {
  console.log("3초 후 실행");
}

console.log("start");
setTimeout(run, 3000);
console.log("end");
```

- setTimeout()이 호출 스택에 쌓였지만, 실행 시 콜백 함수인 run()을 백그라운드로 보내고, 3초 후 테스크 큐로 보내짐
- 이후 호출 스택 실행이 끝나 비워지면 이벤트 루프가 테스트 큐의 run()을 호출 스택에 올림
- run() 실행
