# vitest

[[yarn]] 3와 package.json:module, typescript module 환경에서 바로 적용되서 사용

```sh
yarn add -D vitest vite
```

```sh
yarn vitest # auto watch
yarn vitest run # 1 회용
yarn vitest [name] # regexp 로 대충 맞으면 해당 영역만 실행
```

## error
- [X] ts, js 가 함께 존재하는 경우 js 가 먼저로드되어 타입스크립트 수정후 빌드를 해야하는 문제 해결
  - [[typescript|## build clean]] 참조

## link
- [[typescript]]
- [[test]]
