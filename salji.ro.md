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
  - [ ] **추후** 커넥션이 자주 떨어지는거 같은데 pg 메모리 점검 필요
  - [X] `keyword-notifier-fcm`, `ranker` 재시작으로 복구
    - `fatal` 에러 이후 재시작되지 않는게 **원인**
  - **조치**
    + https://github.com/deptno/salji.ro/pull/628

## link
- [[project]]
