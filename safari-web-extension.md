# safari web extension
> safari web extension 은 swift 파일이 포함된 앱으로 본다
> html, css, javascript 와 manifest.json 을 가지고 타겟 페이지와 통신하는 구조
> manifest 에서 통신 가능한 페이지 및 새 탭에서의 사용 여부를 명시할 수 있다.

## 공통
- `apple-*` 폰트를 사용하면 유저의 폰트 크기 설정에 따라 잘 적응한다

## ios
+ https://developer.apple.com/videos/play/wwdc2021/10104/

- persistant background page 를 지원하지 않는다. -> active page 일때만 background 페이지가 유효
- `onmouse*` 대신 `onpoint*` 이벤트가 사용되며 apple pencil 에 대응가능하다
- windows api 동작이 다르며 몇가지 이벤트(`onRemoved`)가 발생되지 않으니 개발시 확인이 필요하다
  - `browser.windows.create()`
  - `browser.windows.update()`
  - `browser.windows.remove()`
  - `browser.contextMenus`
  - `browser.webRequest`
  - 지원되지 않는 api 와 브라우저에 따라 지원되는 여부가 다른 api 를 사용할때는 항상 `if` 체크를 한다
  - private 모드의 일때와 아닐때를 체크할 할 수 있다.
  - 스플릿뷰가 사용되면 browser.getAll() 의 결과가 늘어난다
- `activeTab` 을 통해서 현재 탭에 대한 권한 부여가 가능하다

## resource
+ https://developer.apple.com/videos/play/wwdc2021/10104/

## link
- [[safari]]
