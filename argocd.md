# argocd

- git 을 single source of truth 로 사용, 상태가 git 과 일치될 수 있는 기능 및 UI 를 제공
- gitops
- helm 지원
- argocd namespace 에서 application crd 를 이용해서 helm 을 매니징
  - 각 헬름 자체는 해당 네임스페이스에 설치 될 것으로 생각됨
- argocd cli 존재

## install
argocd 는 아직 helm chart 를 공식적으로 지원하지 않음, 때문에 manifest 를 통한 설치
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

brew install argocd
```

## error
- traefik tls termination
  + https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#traefik-v22
  - `argocd-server --insecure` 옵션으로 서버를 실행하고 IngressRoute 에서 tls 를 termination

## link
- [[kubernetes]]
- [[git]]
- [[traefik]]
