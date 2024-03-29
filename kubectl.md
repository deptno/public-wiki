# kubectl

## 사용
```sh
kubectl rollout restart [resource type eg.daemonset] [daemonset name] -n [namespace]
kubectl annotate [resouce type] [resource name] kubernetes.io/change-cause="[change-cause will be shown via kubernets rollout history [resource type] [resource name]]"

kubectl rollout history ds/[daemonset name] -n [namespace]
kubectl rollout status ds/[daemonset name] -n [namespace]
# undo 시 환경변수도 해당 시점으로 돌아가는 것으로 보인다
kubectl rollout undo [--to-revision=[revision from history]]
kubectl rollout restart deployment [name] -n [namespace]

kubectl top [resrouce type] [--containers] # cpu, memory usage

kubectl port-forward [pod or service] [localport]:[targetport][]

kubectl auth can-i [kubectl command]

kubectl create secret generic db-user-pass \
    --from-literal=username=admin \
    --from-literal=password=$(openssl rand -base64 10 | tr -d '\n') # eol 제거
    
kubectl create secret generic db-user-pass --from-file=key=file
    
cat file-with-eol | tr -d '\n' | kubectl create secret generic db-user-pass --from-file=key=/dev/stdin # eol 제거
    
kubectl patch secret [name] -p '{"data": {["key"]: "[based encoded value]"}}' 

kubectl api-resources # resource 검색 [[kubernetes-api]] 참조
kubectl api-versions

kubectl get pods --context=[context-name] # context-name 을 주입할 수 있다 i.g. 스크립스 사용시
kubectl cp my-release-master-0:/data/dump.rdb dump.rdb -c redis
```

## link
- [[kubernetes]]
- [[random]]
- [[eol]]
- [[envsubst]]
- [[kubernetes-api]]
- [[shell-script]]
