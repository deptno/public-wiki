# helm

일종의 kubernetes resource 정의를 선언하고 이를 배포하는 개념

```sh
brew install helm
```

```sh
helm repo list # 등록된 repo 리스팅
helm repo add [repo-name] [repo-url]
helm repo update # 차트 리스트 업데이트
helm search repo [repo-name] # chart 검색
helm install [name] -f [values.yaml]
helm ls # 설치된 차트 리스트
helm upgrade [chart-name] -f [values.yaml]
helm delete [chart-name] # 차트 삭제
helm get manifest [chart-name] # 설치 정보
```

- example
```se 
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo search prometheus-community
helm pull prometheus-community/kube-prometheus-stack --untar
# git add kube-prometheus-stack -m "add helm chart: kube-prometheus-stack"
# vim kube-prometheus-stack/values.yaml
helm upgrade prometheus kube-prometheus-stack --install --create-namespace -n prometheus [-f values.yaml]
```

# related
- [[kubernetes]]
