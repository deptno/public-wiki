# kubernetes|쿠버네티스

## pod
- init container
  설정을 위해 선실행되고 종료되는 컨테이너
  + https://kubernetes.io/ko/docs/concepts/workloads/pods/init-containers/
## cronjob
| 분 시 일 월 요일 |            |
|------------------|------------|
| `*/2 * * * *`    | 매 2분마다 |

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
