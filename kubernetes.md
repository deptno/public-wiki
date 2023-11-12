# kubernetes|쿠버네티스

- TODO: sudo kubeadm certs check-expiration
## pod
- init container
  설정을 위해 선실행되고 종료되는 컨테이너
  + https://kubernetes.io/ko/docs/concepts/workloads/pods/init-containers/
  여러 컨테이너가 선언된경우 순차적으로 실행된다[[gpt]]
### multi container pod 의 경우
1. container 중 하나만 죽어도 파드 내의 컨테이너들이 재시작되는 되는 것인지 503 이 뜸
2. 사실 파드 중 하나는 초기 설정을 위한 거였는데 이를 initContainer 로 만들고 나니 에러가 안남
3. 정상 종료되더라도 deployment 특성상 실행중인 container가 유지되어야해서 오류로 보고 pod 재시작이 될 수 있을 것 같음
4. 설정 후 종료되는 파드들은 initContainer 로 옮기자
## cronjob
+ [[crontab]]

| 분 시 일 월 요일 |                         |
|------------------|-------------------------|
| `*/2 * * * *`    | 매 2분마다              |
| `1/2 * * * *`    | 매 2분마다(1분, 3분...) |

- `successfulJobsHistoryLimit: [number]` 옵션을 크게 설정하면 worker node 의 cpu, mem 에 영향을 미친다
- `suspend: [boolean]` 스케줄을 잠시 멈추는 것으로 보이는데 디플로이에할때 유용해 보임
## storage
`StorageSlass` 추가 없이 [[nfs]] mount 가 가능

### pv 
#### access mode
| AccessMode         | 약어 | node:pvc | pvc:pod | 비고    |
| ------------------ | ---- | -------- | ------- | ------- |
| ReadWriteOnce      | RWO  | 1:1      | 1:n     |         |
| ReadOnlyMany       | ROX  | n:1      | 1:n     |         |
| ReadWriteMany      | RWX  | n:1      | 1:n     | eg. nfs |
| ReadWriteOncePod   | RWOP | 1:1      | 1:1     | 1.22+   |
- 약어는 cli 에서 사용
### pvc
- pv:pvc 는 1:1 관계
- pvc 는 namespace 오브젝트임으로 pv 의 access mode 와 무관하게 namespace 에 종속
- ReadWriteOnce 는 **node 기준**으로 pod 는 하나의 pvc에 여럿 붙을 수 있음
  - pvc - pod 1:n 이 가능
  - pv - pvc 는 1:1 ReadWriteOnce 인경우
  - local - pv 는 n:1 이 가능
### error
```sh 
$ kdel pvc [pvc]
persistentvolumeclaim "[pvc]" deleted
^C # 멈춰서 강제 종료

$ k get volumeattachments.storage.k8s.io
No resources found # 사용 주체가 없다

$ pvc get
$ k get pvc
NAME                         STATUS        VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       AGE
[pvc]                        Terminating   pvc-9d3a82a2-49bd-45e7-99b7-e68eb8b76f34   20M        RWO            openebs-hostpath   30h
```
삭제되지 않고 `Terminating` 상태에서 멈춘다
```sh 
kubectl patch pvc {PVC_NAME} -p '{"metadata":{"finalizers":null}}'
```
위 방식으로 삭제가 가능
  + https://github.com/kubernetes/kubernetes/issues/69697#issuecomment-927319274
- [[openebs]] 의 hostpath 인 경우 데이터는 살아 남으니 참고

이때 pv 는 `Released` 상태가 되면 다른 pvc 와는 바인딩할 수 없다. pv 의 상태를 `Available` 로 바꾸기 위해서는 pv 를 수정하여 `claimRef` 를 제거해야한다
```sh 
kubectl patch pv [ PV_NAME ] -p '{"spec":{"claimRef": null}}'
```

---
```sh
  Warning  FailedMount       24s (x7 over 56s)  kubelet            MountVolume.NewMounter initialization failed for volume "pv-static" : path "/var/openebs/local/some-directory" does not exist
```
해당 디렉토리는 수동으로 만들어준다

#### [[nfs]]
```sh 
mount: /var/lib/kubelet/pods/91f95da8-3cea-4f8a-a367-c2b11b3444b5/volumes/kubernetes.io~nfs/test-volume: bad option; for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program.                                                                                                                                 │
```
- [X] worker node 에 nfs-common 설치
```sh 
mount.nfs: failed to apply fstab options
```
- [X] 권한 이슈
#### 0/1 nodes are available
```sh
  Warning  FailedScheduling  29s   default-scheduler  0/1 nodes are available: 1 Too many pods. preemption: 0/1 nodes are available: 1 No
 preemption victims found for incoming pod..
```
무한루프에 의해서 cronjob 의 job 파드가 지속적으로 쌓이는 문제가 있었는데 파드가 160개 정도가되니 뜨질 못한다
- TODO: 노드당 파드 갯수에 대해서 조사
#### disk-pressure
용량 부족이다. 2TB 디스크를 사용 중이었는데 나중에 확인해 보니 우분투 설치시에 100G 파티션을 사용하고 있었다.
그마저도 /var/openebs/local, /var/lib/kubelet/pods/... 에도 데이터가 중첩으로 쌓이면서 50G 만에 발생했다.
- [[lvextend]]
- [[openebs]]

## secret
echo 를 사용하면 newline `\n` 이 붙게된다.
- `echo -n` 을 사용
- `- tr -d '\n'`
```sh 
# 파일로 부터 값 생성
kubectl create secret generic [name] --from-file=[key]=[filename] --from-file=[key]=[filename]
# 직접 입력한 값으로 부터 생성
kubectl create secret generic [name] --from-literal=[key]=[base64 encoded value]
# 기존 시크릿에 추가 혹은 값 변경
kubectl patch secret [name] -p '{"data": {["key"]: "[based encoded value]"}}' 
# 시크릿 네임스페이스 복사
kubectl get secret -n [ namespace ] [ secret name ] | kubectl neat | sed "s/namespace: .*/namespace: [ target namespace]/" | kubectl apply -f -
```

## authentication
+ [[rbac]] 에서 롤 추가에 대한 내용 확인
유저추가
 + https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#normal-user
 
용어정리
| 용어 | 설명                        |
|------|-----------------------------|
| CSR  | Certificate Signing Request |

### CertificateSigningRequest 을 통한 접근
- **주의** 
  - 추후 RoleBinding 과 이어지는 부분은 yaml 안에서의 내용과는 상관없이 인증서 생성시에 지정하는 CN=User / O=Group 와 관계된다
- CertificateSigningRequest 생성
```sh
$ openssl genrsa -out user.key 2048
$ openssl req -new -key user.key -out user.csr
$ cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: user
spec:
  request: $(cat user.csr | base64)
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 3600
  usages:
  - client auth
EOF

certificatesigningrequest.certificates.k8s.io/user created
```
template 화 관련해서는 [[envsubst]] 참조
- CertificateSigningRequest approve
```sh
$ k get csr
NAME     AGE   SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
user   34s   kubernetes.io/kube-apiserver-client   kubernetes-admin   60m                 Pending

$ kubectl get csr user -o jsonpath='{.status.certificate}'| base64 -d > user.crt
$ kubectl config set-credentials user --client-key=user.key --client-certificate=user.crt --embed-certs
User "user" set.

$ kubectl config set-context user --cluster=local --user=user
Context "user" created.

$ kubectl config use-context user
Switched to context "user".

$ kgp
Error from server (Forbidden): pods is forbidden: User "user.dev" cannot list resource "pods" in API group "" in the namespace "default"
``` 
권한이 없어서 되지 않는다 다시 원래의 context(권한이 있는) 돌아와서 role, rolebinding 을 생성한다
- role, rolebinding 생성
```sh
$ kubectl create role test --verb=get --verb=list --resource=pods
role.rbac.authorization.k8s.io/test created

$ kubectl create rolebinding test-user --role=test --user=user
rolebinding.rbac.authorization.k8s.io/test-user created
```
- 다시 user 로 권한을 확인한다
```sh
$ k config use-context user
Switched to context "user".

$ kgp
Error from server (Forbidden): pods is forbidden: User "user.dev" cannot list resource "pods" in API group "" in the namespace "default"

$ kgp -n test
Error from server (Forbidden): pods is forbidden: User "user.dev" cannot list resource "pods" in API group "" in the namespace "test"

$ k auth can-i list pods
no

$ k auth can-i list pods -n test
no
``` 

```sh
$ k api-resources head -1 ; k api-resources | grep pod
NAME                              SHORTNAMES                                      APIVERSION                             NAMESPACED   KIND
pods                              po                                              v1                                     true         Pod
```

부족한 것들을 role, rolebinding 에 넣도록 하자
  - role 에 apiGroups 에 `v1` 추가
  - rolebinding 에 `namespace` 추가
```sh
$ k use-context user
error: unknown command "use-context" for "kubectl"
$ kgp
NAME                                    READY   STATUS      RESTARTS      AGE
curl                                    1/1     Running     5 (10d ago)   23d
```

CertificateSigningRequest 한시간이 지나면 토큰이 만료되어 로그인이 되지 않는다. approve 시점이 아니라 csr 을 생성한 타임으로부터 한시간이다, 생성시 넣어줬던 아래 prop에 의해서 정의된다
```yaml
  expirationSeconds: 3600
```

```sh 
$ kgp
error: You must be logged in to the server (Unauthorized)
```
### ServiceAccount 를 통한 접근
+ https://devopscube.com/kubernetes-kubeconfig-file/

### error
```sh
PEM block type must be CERTIFICATE REQUEST
```
CertificateSigningRequest 생성시에 발생하는데 `cat user.csr | base` 한 값이 들어가야한다

## setup
### local
#### [[minikube]]
로컬에서는 [[minikube]] 를 사용한다.
미니쿠베에서 pv 는 `local` 은 지원되지 않으며 **hostPath** 만 지원한다.
#### [[multipass]]
multipass 를 사용하면 vm을 이용하여 실제와 같은 클러스터 구현이 가능하다.

## error
### annotate
```sh
error: --overwrite is false but found the following declared annotation(s):
```
kubectl annotate 시에 --overwrite 옵션을 추가하거나 --force를 추가해서 해결한다

## link
- [[minikube]]
- [[kubectl]]
- [[helm]]
- [[k9s]]
- [[lens]]
- [[kubespray]]
- [[multipass]]
- [[krew]]
- [[kubeadm]]
- [[calico]]
- [[calicoctl]]
- [[metallb]]
- [[traefik]]
- [[ingress]]
- [[openebs]]
- [[harbor]]
- [[podman]]
- [[metrics-server]]
- [[grafana]]
- [[loki]]
- [[service-account]]
