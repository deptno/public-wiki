# puppeteer

script 로 웹사이트를 런치하여 원하는 동작을 수행한다

## container
container 이미지에서 이를 수행하려면 `chrome --headless` 모드로 실행을 한다
`puppeteer` 대신 `puppeteer-core` 를 이용하고 컨테이너이미지에 chrome 을 다운로드 해놓아야한다

## error
```sh
/app/.yarn/cache/puppeteer-core-npm-19.6.3-fabb100c65-afad54c829.zip/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Frame.js:238
                    ? new Error(`${response.errorText} at ${url}`)
                      ^

Error: net::ERR_NETWORK_CHANGED at https://example.com
    at navigate (/app/.yarn/cache/puppeteer-core-npm-19.6.3-fabb100c65-afad54c829.zip/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Frame
    at process.processTicksAndRejections (node:internal/process/task_queues:95:5)
    at async Frame.goto (/app/.yarn/cache/puppeteer-core-npm-19.6.3-fabb100c65-afad54c829.zip/node_modules/puppeteer-core/lib/cjs/puppeteer/comm
    at async CDPPage.goto (/app/.yarn/cache/puppeteer-core-npm-19.6.3-fabb100c65-afad54c829.zip/node_modules/puppeteer-core/lib/cjs/puppeteer/co
    at async /app/packages/crawler-quasarzone/out/index.js:18:5
```
- puppeteer 의 문제라기보단 container 가 뜬 후에 network 이 변경되면서 생성되는 문제로 보이고 일단 코드 내에서 timeout 처리로 되는 것을 확인했다.
  - 환경은 [[calico]] 를 사용하고 있었다.
  - timeout 은 1초로도 충분했다.

## related
- [[chrome]]
- [[calico]]
- [[kubernetes]]
- [[pod]]
