# 브라우저의 실행 경계와 응답성

같은 계산을 나중에 예약하는 것과 별도 실행 환경으로 옮기는 것은 다르다. 실행 위치·DOM 의존성·전달 비용을 나눠 판단한다.

- **주 스레드의 범위:** 페이지의 일반적인 JavaScript·DOM 조작과 여러 렌더링 작업이 경쟁하는 흐름이다. 긴 동기 계산은 입력 처리와 화면 갱신을 늦출 수 있어, DOM에 “계산 중”을 써도 곧바로 보인다고 보장할 수 없다. 브라우저 전체가 단일 스레드이거나 모든 렌더링이 주 스레드에서 실행되는 것은 아니다.
- **예약과 양보:** `queueMicrotask`는 계산을 다른 스레드로 옮기지 않는다. 체크포인트에서 큐가 빌 때까지 처리하므로 계속 추가되는 마이크로태스크도 다음 일을 늦출 수 있다. `setTimeout(..., 0)`은 즉시 실행이나 실행 전 페인트를 보장하지 않고, `requestAnimationFrame` 안의 무거운 계산도 화면 갱신을 지연시킬 수 있다. task마다 반드시 화면을 그리지는 않는다.
- **Worker의 책임:** DOM 없이 계산할 수 있는 일을 페이지와 분리된 실행 환경으로 옮길 후보가 된다. 부모 페이지 DOM은 직접 조작할 수 없으므로 입력·표시는 페이지에 남기고, 데이터와 결과를 메시지로 주고받는다. 전용 CPU 코어나 별도 프로세스 배정까지 보장하지는 않는다.
- **경계의 비용:** `postMessage`는 동기 함수 호출이 아니다. 일반 데이터 객체는 구조화 복제로 전달되어 데이터 크기에 따른 비용을 고려해야 한다. transfer·공유 메모리 같은 예외도 있어 모든 전달이 복사라고 일반화하지 않는다.
- **응답성과 완료 시간:** 계산 중 입력·피드백을 처리할 여지가 늘어도 전체 완료가 빨라진다는 보장은 없다. 결과 수신과 DOM 갱신은 페이지에 남는다. 병목이 결과 표시라면 계산만 Worker로 옮겨도 그 비용은 없어지지 않는다.

참고: [Chrome의 실행 구조](https://developer.chrome.com/blog/inside-browser-part1), [렌더러](https://developer.chrome.com/blog/inside-browser-part3), [이벤트 루프](https://html.spec.whatwg.org/multipage/webappapis.html#event-loops), [예약 방식](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#microtask-queuing), [Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)
