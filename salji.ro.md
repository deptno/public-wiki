# salji.ro

## 장애

### 0:푸시 X, 랭킹 X
+ [[diary:2024-05-18]]
  - 알람 발송안됨, `ranker` 인지 `redis`, `keyword-notifier-fcm` 인지 재시작, 기록 바로 못해서 기억 없음

### 1:푸시 X, 랭킹 X
+ [[diary:2024-05-20]]
  - [X] redis -> 정상
  - keyword-notifier-fcm
```json
{
  "level":"fatal",
  "file":"/dist/index.js",
  "err": {
    "type":"Error",
    "message":"Connection terminated unexpectedly",
    "stack":"Error: Connection terminated unexpectedly\n    at Connection.<anonymous> (/app/node_modules/.pnpm/pg@8.11.3/node_modules/pg/lib/client.js:132:73)\n    at Object.onceWrapper (node:events:632:28)\n    at Connection.emit (node:events:518:28)\n    at Socket.<anonymous> (/app/node_modules/.pnpm/pg@8.11.3/node_modules/pg/lib/connection.js:63:12)\n    at Socket.emit (node:events:518:28)\n    at TCP.<anonymous> (node:net:337:12)"
  },
  "msg":"dbClient onError"
}
```
  - redis 가 아니라 pg가 떨어짐, fatal 이후 재시작됨
    - pg 가 메모리 치고있음, **3**번 재시작됨
  - [X] **추후** 커넥션이 자주 떨어지는거 같은데 pg 메모리 점검 필요 -> 2x 로 늘림
  - [X] `keyword-notifier-fcm`, `ranker` 재시작으로 복구
    - `fatal` 에러 이후 재시작되지 않는게 **원인**
  - **조치**
    + https://github.com/deptno/salji.ro/pull/628
    - pr head commit 으로 namespace 전체 배포(4개)
    - 선머지
  - **추가**
    - 잘못봤었는데 callback 아안에서 에러핸들러가 호출되면서 종료되지 않는 이유였고 `process.exit` 추가
      + `05205291d63ead0f5f56291e45ade2e6224267a5`
    - [X] 디비 커넥션이 자꾸 끊어지는 문제는 실제로 존재하는 거 같아서 메모리 확장 해야할 것으로 보임(2x)

### 0:푸시 X
+ [[diary:2024-05-21]]
  - 어제배포 중에 `keyword-notifier-fcm` 실서버 배포 누락
  - 
### eomisea.co.kr 
+ [[diary:2024-08-05]]
- 일주일 넘도록 깨짐

## link
- [[project]]
- [[tubemon.io]]
