# openebs

+ https://openebs.io
local node 를 활용하는 storage class

## install
+ https://github.com/openebs/charts
```sh 
kubectl apply -f https://raw.githubusercontent.com/openebs/charts/gh-pages/openebs-operator.yaml
kubectl apply -f https://github.com/openebs/charts/blob/gh-pages/openebs-operator-lite.yaml
```
`kubectl get sc` 로 storage class 생성 확인 가능 openebs namespace 가 함께 생성됨
이후 pv 생성

## usage
- pvc 생성 후 `Pending` 상태는 정상이며 이를 소비할 파드가 마운트 해야지 `Bound` 상태가 된다
- 이 때가 되면 `kubectl get pv` 로 persistent volume 을 확인 가능
- pvc 를 삭제하면 pv 도 함게 삭제된다. 기본 reclaim 정책이 delete 이기 때문인데 바꾸면 수동 pv 삭제 가능
- openebs 는 host node 에 pv 가 존재하기 때문에 bound 된 pvc 가 있는 pod 는 다른 노드에서 뜰 수 없다
- mount path 는 /var/openebs/local 로 보임

## option
- openebs-device - mount 되지 않은 device 를 사용
- openebs-hostpath - 특정 hostpath 를 사용

## 참고
- capacity 에 의한 용량 제한 이상으로 계속 데이터가 쌓임 20M 설정후 34M 확인


## [[error]]
```sh
pod has unbound immediate PersistentVolumeClaims. preemption: 0/1 nodes are available: 1 No preemption victims found for incoming pod..
```
`volumeName` 을 지정하면 파드에 붙지 못한다. 자동으로 할당하도룩 두자

## link
- [[kubernetes]]
- [[book/24-steps-k8s]]
