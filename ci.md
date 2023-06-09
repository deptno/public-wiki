# ci | continuous integration

## cache
### yarn 3
- nodeLinker 를 사용하는 경우
  - `.yarn/cache` 를 캐시하고 `node_modules` 는 캐시하지 않는다
  - 실제 구동시에는 `node_modules` 가 들어가므로 컨테이너 이미지 등을 만들때는 `node_modules` 를 copy

## link
- [[tekton]]
- [[yarn]]
- [[docker]]
