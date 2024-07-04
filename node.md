# node

javascript runtime a.k.a [[nodejs]]

## version 에 따른 업데이트
- 20
  - `.env` 주입 `-node --env-file=config.env. index.js`
    + https://nodejs.org/en/blog/release/v20.6.0
  - `--experimental-permission` 파일 쓰기,  읽기 등이 외부라이브러리등에 의해서 행해질 수 있는 것 봉쇄
    - 해보니 잘안됨
    + https://nodejs.org/en/blog/release/v20.0.0#permission-model
  - buildtin test runner
    - `tsx` 사용, `package.json` 에 `"type": "module"` 이 있어야지 `import` 구문 사용가능
    ```sh 
    node --loader tsx --test **/*.test.ts
    ```
## options
--max-old-space-size
--max-new-space-size
--require [file.js]
--enable-source-maps
--es-module-specifier-resolution=[node] # node 의 경우에는 esmodule 파일의 확장자를 명시하지 않아도 import 가 가능 node@19 부터는 제거될 예정

## esmodule
- __filename -> import.meta.url
- __dirname -> path.dirname(import.meta.url)
 
## 개념
### 비동기 우선 순위
  - single thread
  - async priority
    - micro queue
    - promises
    - timer

## [[error]]
### ECONNRESET
- [[axios]]
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

### ERR_OSSL_EVP_UNSUPPORTED
```sh
Failed to construct transformer:  Error: error:0308010C:digital envelope routines::unsupported
  opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
}
```
node18에서는 실행안되고 node 16에서 실행됨 어떤 것 때문인지는 찾아보지 않음

### Internal Error: spawn Unknown system error -8
package.json 의 script에서 쉘스크립트를 실행하는 경우에 발생
`#!/usr/bin/env bash`
와 같이 최상단 라인에 쉘을 명시해주면 해결된다
```sh
Internal Error: spawn Unknown system error -8
    at ChildProcess.spawn (node:internal/child_process:413:11)
    at Object.spawn (node:child_process:757:9)
    at YK (/Users/deptno/.cache/node/corepack/yarn/3.3.1/yarn.js:4:6994)
    at uh.implementation (/Users/deptno/.cache/node/corepack/yarn/3.3.1/yarn.js:392:17802)
    at uh.exec (/Users/deptno/.cache/node/corepack/yarn/3.3.1/yarn.js:395:1585)
    at uh.run (/Users/deptno/.cache/node/corepack/yarn/3.3.1/yarn.js:395:1756)
    at I7 (/Users/deptno/.cache/node/corepack/yarn/3.3.1/yarn.js:401:6331)
    at process.processTicksAndRejections (node:internal/process/task_queues:95:5)
    at async WRe (/Users/deptno/.cache/node/corepack/yarn/3.3.1/yarn.js:403:16)
    at async o (/Users/deptno/.cache/node/corepack/yarn/3.3.1/yarn.js:403:146)
```

### Automatic extension resolution of the "main" field is deprecated for ES modules.
```sh 
Automatic extension resolution of the "main" field is deprecated for ES modules.
something:build: (node:11541) [DEP0151] DeprecationWarning: Package /Users/deptno/workspace/src/github.com/deptno/salji.ro/backend/packages/logger/node_modules/@internal/constant/ has a "main" field set to "dist", excluding the full filename and extension to the resolved file at "dist/index.js", imported from /backend/packages/logger/dist/logger.mjs.
```
- `package.json` `main` 속성에 `index.js` 와 같이 확장자를 포함한 경로 명시

## 디버깅
### memory leak
- [[env|NODE_OPTIONS]]

```sh
cross-env NODEOPTIONS=--inspect ts-node .
```

`inspect` 옵션으로 덤프를 생성해서 메모리 추적이 가능하다.

#### 디바이스 디버깅
크롬에서 확인이가능
- chrome://inspect/#devices

접속 후 인스펙터를 열면 메모리 탭이 존재하고 여기서 실시간으로 증가와 감소 확인이 가능

## 성능테스트
## 부하 테스트
- [[ngrinder]]

특정 페이지를 지속적으로 해서 tps 측정이 가능하다.  
이와 함께 크롬의 메모리 탭을 확인해서 레코딩을 걸고 부하테스트를 한 후 gc 가 돌고나서도 메모리가 원래되로 돌아오지 않는지 확인이 필요하다.  

# link
- [[javascript]]
- [[nvm]]
- [[npm]]
- [[yarn]]
- [[pnpm]]
- [[axios]]
- [[iconv-lite]]
- [[chrome]]
- [[bun]]
