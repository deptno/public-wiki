# kubernetes|쿠버네티스

- TODO: sudo kubeadm certs check-expiration
## pod
- init container
  설정을 위해 선실행되고 종료되는 컨테이너
  + https://kubernetes.io/ko/docs/concepts/workloads/pods/init-containers/
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
### 0/1 nodes are available
```sh
  Warning  FailedScheduling  29s   default-scheduler  0/1 nodes are available: 1 Too many pods. preemption: 0/1 nodes are available: 1 No
 preemption victims found for incoming pod..
```
무한루프에 의해서 cronjob 의 job 파드가 지속적으로 쌓이는 문제가 있었는데 파드가 160개 정도가되니 뜨질 못한다
- TODO: 노드당 파드 갯수에 대해서 조사

## secret
echo 를 사용하면 newline `\n` 이 붙게된다.
- `echo -n` 을 사용
- `- tr -d '\n'`

## authentication
유저추가
 + https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#normal-user
 
용어정리
| 용어 | 설명                        |
|------|-----------------------------|
| CSR  | Certificate Signing Request |

### CertificateSigningRequest 을 통한 접근
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
  request: $(user.csr | base64)
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 3600
  usages:
  - client auth
EOF

certificatesigningrequest.certificates.k8s.io/user created
```
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

## related
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
