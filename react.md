# react

## error
### server-component
[[nextjs]] 13 에서 server-component 를 사용하고자 하는데 에러가 발생
```sh
TypeError: fetch failed
    at Object.fetch (node:internal/deps/undici/undici:14062:11)
    at process.processTicksAndRejections (node:internal/process/task_queues:95:5)
    at async Home (/Users/deptno/workspace/src/github.com/deptno/things/next-app/.next/server/app/page.js:428:18) {
  cause: Error: connect ECONNREFUSED ::1:3000
      at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1487:16)
      at TCPConnectWrap.callbackTrampoline (node:internal/async_hooks:130:17) {
    errno: -61,
    code: 'ECONNREFUSED',
    syscall: 'connect',
    address: '::1',
    port: 3000
  }
}
[Error: An error occurred in the Server Components render. The specific message is omitted in production builds to avoid leaking sensitive details. A digest property is included on this error instance which may provide additional details about the nature of the error.] {
  digest: '3396887123'
}
```
api 를 localhost 로 접근하면 에러난다 `127.0.0.1` 로 바꿔서 해결, localhost 가 ipv6 로 resolve 되면서 문제가 생기는 것으로 보인다.
 + https://github.com/node-fetch/node-fetch/issues/1624#issuecomment-1233424362
## related
- [[react-native]]
- [[recoil]]
- [[state-management|상태관리]]
