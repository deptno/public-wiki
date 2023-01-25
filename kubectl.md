# kubectl

```sh
kubectl rollout restart [resource type eg.daemonset] [daemonset name] -n [namespace]
kubectl rollout status ds/[daemonset name] -n [namespace]
kubectl port-forward [pod or service] [localport]:[targetport][]
kubectl auth can-i [kubectl command]
```

## related
- [[kubernetes]]
