# axios

## ECONNRESET [[error]]
```sh
Error: Client network socket disconnected before secure TLS connection was established
    at connResetException (internal/errors.js:609:14)
    at TLSSocket.onConnectEnd (_tls_wrap.js:1536:19)
    at Object.onceWrapper (events.js:421:28)
    at TLSSocket.emit (events.js:327:22)
    at endReadableNT (_stream_readable.js:1221:12)
    at processTicksAndRejections (internal/process/task_queues.js:84:21) {
  code: 'ECONNRESET',
```
request 를 보내고 서버쪽에서 응답하기 전에 timeout 처리가 되면 발생하는 에러로 예상
CF 로그에서는 status code 가 000 으로 처리됨

## related
- [[node]]
