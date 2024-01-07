# jwt
JSON Web Token

## 질문
### jwt 를 사용할때 refresh token 이 필요한지?
- 아래와 같은 형태의 사용도 가능하지 않은가?
1. jwt 토큰은 expire 이후에도 decode 자체는 가능하다
2. 만료된 token 을 decode 해서 expire 된 시간을 확인한다
3. 발급**했어야 할** refresh token 의 expire 기간내라면 access token 을 갱신한다
- 위와같은 시나리오는 access token 의 expire 기간을 refresh token 과 같게한것과 같다
- refresh token 은 서버가 아니라 신뢰 클라이언트에 저장되어야한다
  + [[oauth#신뢰, 비신뢰 클라이언트 인증]]
- 안전하게, 만료된 access token 에 만료시에 해당 access token에 대해서 **한번만** 갱신이 가능한 refresh token 을 만들기 위해서는 [[jwt]] 로 만들어서는 안될 것으로 생각된다
  - jwt 는 revoke 가 불가능하기 때문
  - 때문에 이를 성취하기 위해서는 [[jwt]] 를 사용하더라도 db에 저장후에 사용(아무 해시와 다를게 없이 사용)되면 파기하는 방법으로 사용되는게 안정성이 높을 것으로 생각된다
- 이를 분리하는건 탈취에 대한 방어인데 access_token 과 refresh_token 을 같은 비신뢰 저장소에 저장해 둔다면 동일하게 탈취되므로 의미가 없다
- 구현이슈는 복잡하고 생각할게 많으므로 [[mvp]] 를 구현하는데 방해가 될 수 있으므로 보안 요구사항을 생각하고 진행하는 것이 좋겠다

#### refresh token 구현 아이디어
- 클라이언트에 저장 하지 않고 access token 에 대해서 한번만 발생할 수 있는 refresh token 발급 api 를 생성한다
```mermaid
sequenceDiagram
  actor app as app or web
  participant service as service server
  participant auth as auth server
  participant db as database

  app ->> auth: 인증 요구
  auth ->>+ app: access_token
  app --> app: expired
  app ->>- service: request with expired auth
  service ->> app: 401
  app ->> auth: /auth/refresh/token=[access_token]
  auth --> auth: if 만료 &&  만료시간이 특정 기간이내
  auth -->> db: find access_token
  db -->> auth: db에 없음
  note right of db: db에 있는 경우 이미 발행된 케이스
  auth -->> db: save access_token
  auth --> auth: renew access_token
  auth ->> app: access_token
```
  - 이런 구현인 경우 만료된 토큰을 가지고 /auth/refresh 를 지속적으로 요구하면 db 가 지속되어 기존 스펙보다 나은게 없는 것 같다
  - refresh token 을 함께 발급하고 이를 안전한 저장소에 클라이언트에 저장하는게 요구사항인만큼 **이것이 관철되는것이 좋을것**으로 생각한다
  - 최종적으로 구현을 정리하여 단순화하면 아래와 같다
```mermaid
sequenceDiagram
  title: 리프레시 토큰교환 한번만 진행
  actor app as app or web
  participant be as backend
  participant db as database

  app ->> be: 인증 요구
  be ->> db: save refresh_token with user
  be ->>+ app: access_token, refresh_token
  app --> app: expired
  app ->>- be: expired token
  be ->> app: 401
  app ->> be: /auth/refresh?access_token&refresh_token
  be --> be: if refresh token 만료되지 않은 경우
  be ->> db: try to delete refresh_token
  db --> app: 401: db에 이미 없는 경우 이미 교환한 케이스
  db ->> be: 삭제 성공: 토큰이 있는 경우
  be --> be: renew access_token, refresh_token
  be ->> db: save refresh_token with user
  be ->> app: access_token, refresh_token
```
  - access token 을 salt 로 써도 좋을 듯

#### 하이브리드 웹
- 토큰 갱신을 어디서할 것인지에 따라서 보안레벨이 달라진다
- **인증된 SSR 사용시** 에는 401 화면 노출을 피할 수 없다
- 웹뷰에서 갱신을 하게 되면 중복 구현이 발생한다

##### 갱신시 webview 를 새로 고침
```mermaid
sequenceDiagram
  actor app as app
  actor wv as webview
  participant be as backend
  participant db as database

  app ->> be: 인증 요구
  be ->> db: save refresh_token with user
  be ->> app: access_token, refresh_token
  app ->> wv: open with access_token
  wv ->> be: expired token
  be ->> wv: 401
  wv ->> app: 401
  app ->> be: /auth/refresh?access_token&refresh_token
  be --> be: if refresh token 만료되지 않은 경우
  be ->> db: try to delete refresh_token
  db --> app: 401: db에 이미 없는 경우 이미 교환한 케이스
  db ->> be: 삭제 성공: 토큰이 있는 경우
  be --> be: renew access_token, refresh_token
  be ->> db: save refresh_token with user
  be ->> app: access_token, refresh_token
  app ->> wv: open with access_token
```
- weview 를 열때 access_token 을 전달
- 만료시에는 app 에서 토큰 리프레시를 담당하고 웹뷰를 새로고침한다 
- 구조가 단순한 대신 새로고침 되므로 경험이 떨어진다, 스크롤 위치 이슈

##### 갱신시 webview 에서 갱신후 후 앱에 전달 
```mermaid
sequenceDiagram
  actor app as app
  actor wv as webview
  participant be as backend
  participant db as database

  app ->> be: 인증 요구
  be ->> db: save refresh_token with user
  be ->> app: access_token, refresh_token
  app ->> wv: open with access_token
  wv ->> be: expired token
  be ->> wv: 401
  wv ->> app: get refresh_token
  app ->> wv: refresh_token
  wv ->> be: /auth/refresh?access_token&refresh_token
  be --> be: if refresh token 만료되지 않은 경우
  be ->> db: try to delete refresh_token
  db --> app: 401: db에 이미 없는 경우 이미 교환한 케이스
  db ->> be: 삭제 성공: 토큰이 있는 경우
  be --> be: renew access_token, refresh_token
  be ->> db: save refresh_token with user
  be ->> wv: access_token, refresh_token
  wv ->> app: store access_token & refresh_token
  wv ->> be: retry failed api
```
- webview 의 새로고침은 발생하지 않는다, 무한 스크롤 등 구현이 다소 유리
- app 에서도 로직이 존할 것이므로 full webview 기반이 아니라면 refresh token 로직은 중복구현된다
- 좀더 seamless 한 경험을 줄 수 있지만 **인증된 SSR** 사용이 안되는건 마찬가지
- ssr 은 포기해야지 싶다

##### 앱에서 갱신 + 웹뷰에 통신으로 브로드 캐스팅
```mermaid
flowchart LR
app --"5: injectJavascript(access_token)"--> webview0
app --"5: injectJavascript(access_token)"--> webview1
app --"5: injectJavascript(access_token)"--> webview2
app --"5: injectJavascript(access_token)"--> webview3
  webview3 -- 2: 401 --> app
  webview3 -.-> service
  service --1: 401--> webview3
  app --3: /refresh/token --> service
  service --4: access token --> app
  webview0 --6: 갱신--> webview0
  webview1 --6: 갱신--> webview1
  webview2 --6: 갱신--> webview2
  webview3 --6: 갱신--> webview3
```
- ssr 을 포기하면 가장 심리스한 구현이 아닐가 싶

## link
- [[oauth]]
- [[jwe]]
