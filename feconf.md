# feconf

## 2023

### react native, [metro](metro) 를 넘어서
+ https://www.youtube.com/watch?v=QfU5REp8sjQ
- rn 은  하나의 js 파일만 받아들임 + [hermes](hermes)
- bundler 하는 일
  - resolution: 파일 경로 처리
  - load: ts -> js
  - optimization: tree shaking
- [esbuild](esbuild): 결과적으로 속도 10 향상 용량 1/3으로 줄어듬, `--reset-cache` 같은 일관성없는 옵션 필요 없어짐
