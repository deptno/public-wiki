# dify

## install
- [[helm]]
  ```sh
  helm repo add dify https://borispolonsky.github.io/dify-helm
  helm repo update
  helm pull dify/dify --untar
  # cwd, directory 변경
  helm install dify chart/dify@0.22.0 --create-namespace -n dify
  # route 53 등록 및 ingress 설정
  ```
+ https://github.com/deptno/cluster-amd64/pull/1

## link
- [[ai]]
- [[n8n]]
