# metrics-server

```sh
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
helm upgrade --install metrics-server metrics-server/metrics-server
```

```sh
kubectl top nodes
```

## error
```sh
$ kubectl top nodes
Error from server (ServiceUnavailable): the server is currently unable to handle the request (get nodes.metrics.k8s.io)
```

```log 
E0116 14:56:01.625532       1 scraper.go:140] "Failed to scrape node" err="Get \"https://192.168.0.x:10250/metrics/resource\": x509: cannot vali
date certificate for 192.168.0.x because it doesn't contain any IP SANs" node="5950x"
I0116 14:56:07.963441       1 server.go:187] "Failed probe" probe="metric-storage-ready" err="no metrics to serve"
```

helm chart 의 values.yaml 에서 `defaultArgs` 에 아래 옵션을 추가한다

```sh
--kubelet-insecure-tls
```


## link
- [[kubernetes]]
- [[k9s]]
- [[tekton]]
