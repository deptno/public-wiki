# ci | continuous integration

## 생각
- [[diary:2023-12-10]] self hosted ci 서버를 구축할 때, binary 를 포함하고 있어 [[git]] 의 레포지터리 용량이 크다고한다면 self hosted 에서 해당 브랜치를 지속적으로 패치하고 이를 클론 혹은 참조하는게 유리할 것이라는 생각이 든다

## cache
### yarn 3
- nodeLinker 를 사용하는 경우
  - `.yarn/cache` 를 캐시하고 `node_modules` 는 캐시하지 않는다
  - 실제 구동시에는 `node_modules` 가 들어가므로 컨테이너 이미지 등을 만들때는 `node_modules` 를 copy

## link
- [[tekton]]
- [[yarn]]
- [[docker]]
- [[cirrus]]
