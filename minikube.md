# minikube

로컬에서 사용된다.
대체재 로는 [[k3s]], [[k0s]], [[kind]] 등이 있다.
- https://minikube.sigs.k8s.io/docs/start/

```sh
brew install minikube
brew install --cask docker 
```

docker.app 실행 후에는 퍼미션 설정

```sh
minikube start
minikube pause
minikube unpause
minikube stop
```

## service 접근
service 에 접근하기 위해서, `ingresss` 가 없는 경우 `service` 를 `LoadBalancer` 타입으로 노출한다.
```sh
minikube tunnel
```
을 실행해 줘야지만 접근이 가능하다. 주소는 **localhost:port** 로 열리게 된다.
[[jobs]] 나 [[tmux]] 를 이용하면 터미널 창 하나에서 가능하다.

## pv
`local` 은 지원되지 않으며 **hostPath** 만 지원한다.
- https://minikube.sigs.k8s.io/docs/handbook/persistent_volumes/

### storage class
특정이름의 스토리지 클래스가 없다고 나오는 경우 생성해야 된다.

```sh
kubectl get storageclass
NAME                 PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
standard (default)   k8s.io/minikube-hostpath   Delete          Immediate           false                  2d3h
```

`storage-classes.yaml` 작성
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: k8s.io/minikube-hostpath
reclaimPolicy: Retain
```

```sh
kubectl apply -f storage-classes.yaml
```


## link
- [[kubernetes]]
- [[docker]]
- [[jobs]]
- [[tmux]]
