# github-action

## self-hosted-runner
1. arc 설치
  + https://docs.github.com/en/actions/tutorials/use-actions-runner-controller/quickstart
  - github app
    - app 생성, webhook deactive, 권한은 문서 참조
      + https://docs.github.com/en/actions/tutorials/use-actions-runner-controller/authenticate-to-the-api#deploying-using-personal-access-token-classic-authentication
    - app id, installation id(install 후 주소창의 마지막 숫자), pem 키로 secret 생성

## 설치순서 정리
- helm 으로 gha-runner-scale-set{,-controller} 으로 chart/ 디렉토리 `pull` 한다고 가정
- helm 으로 gha-runner-scale-set-controller 를 먼저 설치 namespace: arc-systems 
```sh
helm upgrade -i arc chart/gha-runner-scale-set-controller@0.12.1 -n arc-systems
```
- [[github]] 과의 인증을 위해 PAT 발급, github apps 을 통한 발급 방법이 추천된다
  + https://docs.github.com/en/actions/tutorials/use-actions-runner-controller/authenticate-to-the-api#deploying-using-personal-access-token-classic-authentication
  - github apps
    - Homepage URL: https://github.com/actions/actions-runner-controller
    - Private keys 생성
  - app 생성 및 인스톨
    - app id, installation id(install 후 주소창의 마지막 숫자), pem 키로 secret 생성
```sh 
 kubectl create secret generic pre-defined-secret \
   --namespace=arc-runners \
   --from-literal=github_app_id=[GITHUB_APP_ID] \
   --from-literal=github_app_installation_id=[GITHUB_APP_INSTALLATION_ID] \
   --from-literal=github_app_private_key='[----BEGIN RSA PRIVATE KEY -----...]'
```
  - helm install gha-runner-scale-set 설치
  - 상황에 맞춰 chart value 수정
```sh
helm upgrade -i arc-runner-set chart/gha-runner-scale-set@0.12.1 -n arc-runners \
  --set githubConfigUrl="https://github.com/org" \
  --set githubConfigSecret="pre-defined-secret"
```
- `githubConfigUrl` 은 `org` 혹은 `username/repository` 두개만 가능한 것같다
- `0.13.0` 까지 확인

## 여러개의 gha-runner-scale-set
- 구조
  - gha-runner-scale-set-controller 이 모니터링
  - gha-runner-scale-set 가 새로 발행되는 경우 여기서 github 관련 설정을 읽어서 arc-systems 각 scale-set 별 리스너를 만듬
  - gha-runner-scale-set-controller 하나만 존재 gha-runner-scale-set 은 여러개 존재 가능, 네임스페이스 동일 유지 가능
```sh
helm upgrade -i arc chart/gha-runner-scale-set-controller@0.13.0 -n arc-systems
helm upgrade -i deptno-codex-mon chart/gha-runner-scale-set@0.13.0 -n arc-runners \
  --set githubConfigUrl="https://github.com/username/repository" \
  --set githubConfigSecret="user-secert"
helm upgrade -i deptno-ai chart/gha-runner-scale-set@0.13.0 -n arc-runners \
  --set githubConfigUrl="https://github.com/org" \
  --set githubConfigSecret="org-secret"
```

## link
- [[ci]]
- [[github]]
- [[self-hosted]]
