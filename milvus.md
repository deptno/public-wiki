# milvus

## 설치
+ https://milvus.io/docs/install_cluster-helm.md#Install-Helm-Chart-for-Milvus

```sh 
helm repo add milvus https://zilliztech.github.io/milvus-helm/
helm repo update
helm pull milvus/milvus --untar
mv milvus milvus@[VERSION]
helm upgrade -n [NAMESPACE] --install milvus milvus@[VERSION]
```

## link
- [[ai]]
- [[helm]]
