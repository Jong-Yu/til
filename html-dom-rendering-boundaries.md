# HTML·DOM·화면과 스크립트 실행 시점

받은 HTML 소스, 메모리의 DOM, 렌더링된 화면은 서로 다른 상태다. DOM을 찾거나 수정했다고 표시 여부와 화면 갱신 시점까지 확정되는 것은 아니다.

- **소스와 DOM:** 브라우저는 HTML을 파싱해 DOM을 만든다. JavaScript가 `textContent`를 바꾸면 현재 DOM의 텍스트가 달라지지만, 그 동작만으로 서버의 HTML 파일이 수정되지는 않는다.
- **DOM과 표시:** `display: none`이 적용되면 요소와 그 내용의 표시 박스를 만들지 않아 화면의 자리도 사라진다. DOM 요소는 남으므로 여전히 찾고 읽을 수 있다. 실제 표시 여부는 클래스 이름만이 아니라 최종 적용 스타일로 판단한다.
- **수정과 렌더링:** 스타일 계산은 적용 규칙, 레이아웃은 박스의 크기·위치, 페인트는 그릴 내용, 합성은 레이어를 합친 결과를 다룬다. 변경마다 모든 단계가 반복되지는 않는다. 긴 JavaScript 작업이 화면 갱신을 지연시킬 수 있고 여러 DOM 변경이 함께 반영될 수 있어, 코드 각 줄의 결과가 반드시 차례로 보이지는 않는다.
- **스크립트와 DOM 준비:** 초기 HTML의 외부 일반 스크립트에서 `async`·`defer`가 없으면 가져오기와 실행이 파싱을 막는다. `defer`는 파싱 완료 뒤 실행하므로 뒤쪽 요소에 접근할 수 있다. `async`는 준비되면 실행하며 파싱 완료를 보장하지 않는다. 모듈·동적 삽입 스크립트에는 이 설명을 그대로 적용하지 않는다.
- **DOMContentLoaded:** 파싱과 지연 스크립트 실행 뒤의 이정표다. 이미지·하위 프레임·async 스크립트 완료나 최종 화면 완성을 보장하지 않는다. 스타일시트를 직접 기다리지는 않지만, 지연 스크립트가 스타일시트를 기다리면 간접적으로 늦어질 수 있다.

참고: [브라우저 로딩](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites), [script 규칙](https://html.spec.whatwg.org/multipage/scripting.html#the-script-element), [DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event), [렌더링](https://web.dev/articles/rendering-performance), [박스 생성](https://drafts.csswg.org/css-display/#box-generation)
