# argo-workflows

## 설치
+ https://artifacthub.io/packages/helm/argo/argo-workflows
+ https://github.com/deptno/cluster-amd64/compare/02a20c3...d7a5037
- 위 커밋 반드시 참조
- serviceAccount + ns, workflowsDefaults 설정이 있어야지 동작
- ui 인증 제거,  서버모드
```sh 
# https://github.com/argoproj/argo-helm
helm repo add argo https://argoproj.github.io/argo-helm
```

## link
- [[argocd]]
