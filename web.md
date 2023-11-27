# web

## Q. iframe 안에서의 스크롤 이벤트를 막고 parent element 를 대신 스크롤
```html
<iframe />
<div class="event-hooker"/>
```
- 형태로 구성하는경우 `event-hooker` 에 `pointer-events: none` 을 주입하면 아무런 이벤트도 트리거 되지 않는다
```html
<div class="event-wrapper">
  <iframe />
</di>
```
- `event-wrapper` 에서 특정 이벤트는 `stopPropagation` 으로 통과시키지 않는다(캡쳐링 단계)
- 해보니 *iframe* 인 경우는 이벤트를 부모보다 먼저 받는 것으로 보여서 해결되지 않는다
```html
<div class="target" />
<iframe />
```
- iframe 의 특정 이벤트를 `target` 으로 토스한다
  ```js
  iframe.contentWindow.body.onwheel = (ev) => target.dispatchEvent('wheel', ev)
  ```
  - cross-origin 인 경우 에러 발생
    - Uncaught DOMException: Blocked a frame with origin "http://localhost:4000" from accessing a cross-origin frame.
    - postMessage 로 우회를 해야할 것으로 보임
  

## links
- [[css]]
- [[pwa]]
