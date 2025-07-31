# keycloak
- [[helm]] 차트 설치 매우 간단
- 설정은 아래 시리즈 참조
  + https://devocean.sk.com/blog/techBoardDetail.do?ID=166983#none

## admin 설정
- 로그인 이후 어드민 id 새로 생성 후 기존 role 복사해서 넣고 임시 계정은 삭제, 보안이슈
- 새로운 계정으로 프로젝트 렐름 생성
- 해당 렐름에 프로젝트 생성
- 로컬 개발환경에서 로그인 테스트하려면 redirect uri 에 `http://localhost:3000/*` 형식 추가 필요
- [[#identity providers]] 에서 social login 추가 가능
  - 해당 로그인해도 필수 파람 요구받

## :identity providers:
- social login 추가 가능 추가시에 로그인 화면에 추가됨
  - 해당 idp 에서 client id, key 발급 필요
  - social login 에 추가할때 나오는 redirect uri 도 추가해야함
    - [[#first-login-flow]] 에서 수정가능

### :first-login-flow:
- 최초 로그인시 부족한 prop등을 채우기 위해 실행되는 flow
  - 예: 카카오 로그인 후 정보입력 요구 진행

## link
- [[oauth]]
- [[zitadel]]
