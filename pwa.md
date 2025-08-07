# pwa | progressive web app

+ https://developer.mozilla.org/ko/docs/Web/Progressive_web_apps/Tutorials/js13kGames/Introduction
+ https://web.dev/i18n/ko/progressive-web-apps/

## 컨셉
- `manifest.js` 를 통해 앱처럼 보일 수 있는 아이콘등의 정보를 제공한다
- service worker(일종의 쓰레드로 이해) 를 지원하는 브라우저에서 동작하는 worker 파일을 등록
- install - cache 등록, push notification, response hooking 등을 제공
- 지원되는 브라우저를 이용하는 점에서 (설치된)  system webview 를 사용하는 [[tauri]] 와 닮은 점이 있다

## native wapping 시 고려할점
- pwa 는 install 을 통해 home screen 에 설치된다
  - [ ] native 래퍼로 설치된경우 해당 install 은 동작하면안된다 [[todo]]
    - 이경우에도 캐시 이점이 있는것인지?
- app store 를 통해서 검색가능한 장점은 pwa 와는 무관
- push notification 의 경우도 pwa 자체  가지고 있지만 native 도 가지고 있어서 얻는 이점은 없다
- app store 와 pwa 로 유저가 분산되면서 app store 의 노출 기준일수도 있는 다운로드수에는 악영향이 있을 수 있다

## [[nextjs]]
- [[diary:2025-08-07]]
- svg를 먼저 작성, generateFavicon 하면 배경을 잃어버림, download 로 svg 다운로드
  + https://realfavicongenerator.net
- 다운로드 받은 svg 삽입후 생성, margin 0 줬음
  - https://www.pwabuilder.com/imageGenerator
- file 위치
  ```sh
  .
  ├── public
  │   ├── web-app-manifest-192x192.png
  │   └── web-app-manifest-512x512.png
  ├── src
  │   └── app
  │       ├── icon.svg
  │       ├── layout.tsx
  │       ├── manifest.ts
  │       ├── robots.ts
  │       └── sitemap.ts
  └── tsconfig.json
  ```

### 드는 질문 [[todo]]
- [ ] install 과정을 생략하고도 특정 파일을 캐싱할 수 있는지?
- [ ] pwa wrapper 를 이용하는 이유
  - [ ] 하이브리드 앱을 만들고 싶을 뿐인데, 웹에서도 접근할 수 있는 경우에 이를 단지 pwa로 만든 경우
    - 다만 이 경우에도 cache 를 제외한 install 등의 기능은 disable 시켜야 할 것
  - [ ] 하이브리드 앱의 장점을 취하면서도 데이터 사용량등 네트워크 트래픽 최소화를 위함
- [ ] *중요* pwa 가 아닐시에도 하이브리드앱일때 웹서버의 장애가 난 경우에대한 우아한 핸들링 가능 여부

## link
- [[web]]
- [[tauri]]
- [[nextjs]]
- [[safari]]
