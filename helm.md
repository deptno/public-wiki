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

helm upgrade CHART_NAME CHART_PATH [-f values.yaml] [--debug] [--dry-run] [--namespace NAMESPACE] [--create-namespace]
helm upgrade -i [chart-name] . -f [values.yaml] # 없으면 인스톨, 이걸 주로 쓰게됨
helm upgrade --install [chart-name] path/to/chart [-f values.yaml]
helm upgrade --install [chart-name] path/to/chart [-f values.yaml]

helm delete [chart-name] # 차트 삭제

helm get manifest [chart-name] # 설치 정보
```

## option
- debug - verbose 로그
- dry-run - 테스트

- example
```sh
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo search prometheus-community
helm pull prometheus-community/kube-prometheus-stack --untar
# git add kube-prometheus-stack -m "add helm chart: kube-prometheus-stack"
# vim kube-prometheus-stack/values.yaml
helm upgrade prometheus kube-prometheus-stack --install --create-namespace -n prometheus [-f values.yaml]
```

## error
- [[postgresql]]

# link
- [[kubernetes]]
- [[postgresql]]
