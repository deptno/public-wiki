# prometheus

kube-prometheus-stack [[helm]] chart 로 설치시에 [[grafana]] [[alert-manager]] 까지 함께 설치된다.

```sh
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo search prometheus-community
helm pull prometheus-community/kube-prometheus-stack --untar
# storageClass 등 설치 필요
helm upgrade --install --create-namespace -n prometheus . 
```

## related
- [[kubernetes]]
- [[grafana]]
- [[loki]]
- [[promtail]]
