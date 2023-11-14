# bruno

+ https://www.usebruno.com

## feature
- [[postman]] 대체제, api collection 을 자체 포맷인 `.bru` 를 통해 저장한다.
  - file 로 저장하기 때문에 [[git]] 등 vcs 를 통한 공통 관리가 가능하다
- 장황한 하다 주장하는 json 을 대신해 http 에 최적화된 포맷을 택했다.
  - diff 등을 통해서 쉽게 파악 가능
- environments, secret 등을 지원, 변수화에 대한 것
- pre, post scripting([[javascript]]) 을 지원하며 알만한 라이브러리(lodash, moment 등)을 빌트인으로 제공한다
- 자체 테스트를 지원한다

## 생각
- neovim 이 초기 extension 리스트업에서 빠진상태
- 기본적으로 gui 프로그램

### 장점
- 백엔드 관점에서는 test를 지원해서 ci/cd 파이프라인에서는 편리하게 이용가능할 것으로 보임

### 단점
- 초기버전인 `.bru` 자체 포맷이 기존 플러그인들의 포맷대비 장점이 뚜렷하지않다
  - [[intellij]] 의 자체 플러그인인 `HTTP Client` 에도 비슷기능을 이미 지원하고있다.
  - [[vim]] 쪽에도 유사한 플러그인이 오래전부터 존재했다.
    + https://github.com/diepm/vim-rest-console
- 프로젝트가 초기라는점

## link
- [[postman]]
- [[curl]]
