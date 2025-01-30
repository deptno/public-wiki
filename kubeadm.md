# kubeadm
+ https://kubernetes.io/docs/reference/setup-tools/kubeadm/

## 인증서 갱신 기록
- [X] DONE: kubeadm certs check-expiration [[diary:2023-12-30]]
- [ ] TODO: kubeadm certs check-expiration [[diary:2024-12-20]]

## k8s cluster 생성

최종적으로는 성공한 스펙은 아래와 같다
- control-plane 1 : arm64 raspberry-pi 4 4gb
- worker-node 3 : arm64x2 (raspberry-pi 4 8gb + raspberry-pi 4 4gb) + x86_64x1
- cni : calico
- os: ubuntu 22.04.1 lts jammy

```sh 
NAMESPACE     NAME                                      READY   STATUS    RESTARTS          AGE
kube-system   calico-kube-controllers-7bdbfc669-xm6ps   1/1     Running   187 (3h29m ago)   36h
kube-system   calico-node-44zs8                         1/1     Running   0                 43m
kube-system   calico-node-fxzwn                         1/1     Running   82 (4h ago)       36h
kube-system   calico-node-v4d8w                         1/1     Running   0                 19m
kube-system   calico-node-w57c2                         1/1     Running   0                 34m
kube-system   coredns-787d4945fb-ngrmj                  1/1     Running   110 (3h29m ago)   2d12h
kube-system   coredns-787d4945fb-p4mgs                  1/1     Running   124 (3h26m ago)   2d12h
kube-system   etcd-pi0                                  1/1     Running   787 (56m ago)     2d13h
kube-system   kube-apiserver-pi0                        1/1     Running   765 (55m ago)     2d13h
kube-system   kube-controller-manager-pi0               1/1     Running   756 (52m ago)     2d13h
kube-system   kube-proxy-7mgj9                          1/1     Running   0                 43m
kube-system   kube-proxy-knl9w                          1/1     Running   610 (55m ago)     2d13h
kube-system   kube-proxy-p7fhv                          1/1     Running   0                 34m
kube-system   kube-proxy-qjdrl                          1/1     Running   8 (24m ago)       42m
kube-system   kube-scheduler-pi0                        1/1     Running   792 (52m ago)     2d13h
```
재시작을 보면 알겠지만 엄청나다
`kube-*` pods 들과 kubelet 이 계속 내려갔는데 /etc/containerd/config.toml 이 직접적인 영향을 미쳤다.
kubelet 은 containerd 설정이 완전히 마쳐지면 kubeadm 통해서 실행해야한다. `systemctl status kubelet.service` 의 보이는 환경변수들이 없는 경우
`ufw` 는 pi2 에 enable 되어있으나 정상적으로 동작중이다. 모니터링을 해당 것을 enable 처리 유지중이다 - kubespray 에서는 disable 권장
- [ ] TODO: etcd 다중화
- [ ] TODO: 외부 접속

### 생성 후 조인
+ https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/adding-linux-nodes/
- master node 에서 토큰 발급한다
```sh 
kubeadm token create --print-join-command
```
- join 하고자 하는 node 에서 붙여 넣는다
- worker node 로 조인
```sh 
sudo kubeadm join 192.168.0.7:6443 --token [token] --discovery-token-ca-cert-hash [sha256]
```
- master node 로 조인
```sh 
sudo kubeadm join 192.168.0.7:6443 --token [token] --discovery-token-ca-cert-hash [sha256] --control-plane
```
  아래와 같은 에러가 발생한다
####unable to add a new control plane instance to a cluster that doesn't have a stable controlPlaneEndpoint address 
```sh 
error execution phase preflight:
One or more conditions for hosting a new control plane instance is not satisfied.

unable to add a new control plane instance to a cluster that doesn't have a stable controlPlaneEndpoint address

Please ensure that:
* The cluster has a stable controlPlaneEndpoint address.
* The certificates that must be shared among control plane instances are provided.


To see the stack trace of this error execute with --v=5 or higher
```
```sh 
pi@pi0:~$ sudo kubeadm join 192.168.0.7:6443 --token 8yau5m.x2sn6km9bbzszspa --discovery-token-ca-cert-hash sha256:68d77ed663014c8ee6c8b5fabff16aed05113eb821cc3c745cd0bb4bbff8daeb --control-plane --apiserver-advertise-address=192.168.0.7
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
error execution phase preflight:
One or more conditions for hosting a new control plane instance is not satisfied.

unable to add a new control plane instance to a cluster that doesn't have a stable controlPlaneEndpoint address

Please ensure that:
* The cluster has a stable controlPlaneEndpoint address.
* The certificates that must be shared among control plane instances are provided.


To see the stack trace of this error execute with --v=5 or higher
```
---
#### [[diary:2025-01-31]]
- [[wsl]] 환경에서 진행
- 원본 문서 참조
  + https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/adding-linux-nodes/
- control plane
  - 접속해서 token 및 ca-cert-hash 생성
- 추가될 [[wsl]] worker node
  - [[kubeadm]] 설치
  - join 명령어에다가 control plane 에서 생성한 키와 함께 접속
  - 에러 발생
    - host os 인 [[windows]] 에서 방화벽 내림
    - `apt-get` 으로 `containerd` 설치
    - `swap` off
    - `cgroup2` 설정
```sh
# 에러 메시지
[ERROR FileContent--proc-sys-net-ipv4-ip_forward]: /proc/sys/net/ipv4/ip_forward contents are not set to 1
# 해결
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
# 확인
cat /proc/sys/net/ipv4/ip_forward # 결과 1
```
    - 이외 wsl 이기 때문에 포트 문제가있을 것 같아서 windows 에서 방화벽 내림
  - 비 상시 node 라면 `taint` 설정

## amd64 설치
+ 2023-01-14 

1. ubuntu 22.04.5 amd64 설치하면서 upgrade + openssh 로 설치
```sh 
# password 없는 접근을 위해 [[ssh-copy-id]] 를 통한 key 복사 
ssh-copy-id -i ~/.ssh/[key].pub [server] 

sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

# off swap
# +2023-01-24 TODO: [[sed]] 는 부팅시 스왑 파티션 마운트를 막으려는 것 같지만 현재 맞지 않는 것으로 보임 아래 swap 을 처리해야 할 것으로 보임
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
sudo swapoff -a

# containerd
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml

sudo sysctl --system
sudo systemctl restart containerd

cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

# lsmod 경우가 없는 경우 modprobe
lsmod | grep overlay
lsmod | grep br_netfilter
sudo modprobe overlay
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

sudo sysctl --system
sudo systemctl restart containerd

sudo kubeadm config images pull
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 

# cni
# + https://projectcalico.docs.tigera.io/getting-started/kubernetes/helm
helm repo add projectcalico https://projectcalico.docs.tigera.io/charts
kubectl create namespace tigera-operator
helm install calico projectcalico/tigera-operator --version v3.24.5 --namespace tigera-operator
watch kubectl get pods -n calico-system

kubectl edit ippools.crd.projectcalico.org default-ipv4-ippool`
# ipipMode 값을 Always -> CrossSubnet 으로 변경

k create ns traefik
helm install traefik traefik/traefik -n traefik
keno
```

swapoff 는 아래 명령어를 통해서 확인 가능하며 `sudo swapoff -a` 를 하면 리스트에 보이지 않는 것으로 확인이 가능하다
- `cat /etc/swaps`
- `free -h`
- `blkid`
```sh 
$ cat /proc/swaps
Filename                                Type            Size          Used             Priority
/swap.img                               file            8388604       0-2
```
taint 를 제거해서 스케쥴 가능하게 만들면 traefik 파드가 뜨는데 성공한다
```yaml
  taints:
  - effect: NoSchedule
    key: node-role.kubernetes.io/control-plane
```

이후에는 쉘에 나오는 내용따라치고 KUBECONFIG 를 설정하면 사용이 가능하다.
[[ingress]] 를 설정해서 사용을 시작하면된다.

## error
```sh
pi@pi0:~$ kubectl get pods

The connection to the server [ip]:6443 was refused - did you specify the right host or port?
```
[[kubeadm]] init 이후에 6443 refused 가 나오는 경우
```sh
nc -v [ip] 6443
```

결과가 없는 것이 확인 되면 서비스를 재시작한다
```sh
sudo systemctl restart kubelet.service
```

kubelet 이 계속 죽는다면 containerd 를 의심해야한다.
containerd 가 모두 셋업되면 kubelet 을 restart 하는게 아닌 kubeadm init 을 해줘야한다

swapoff 를 안한 경우 문제가 될 수 있으니 참조한다 + [[raspberry-pi]]
```sh
sudo -i
sudo swapoff -a
```
+ https://d-life93.tistory.com/437?category=1013222
- [ ] 같은 에러가 계속 나는 이유는 apiserver pod 가 죽기 때문이면 관련글 읽어보기
  + https://www.cisco.com/c/ko_kr/support/docs/wireless/ultra-cloud-core-session-management-function/217777-troubleshoot-continuous-restart-of-kube.html
```sh
pi@pi0:~$ kubectl get events
LAST SEEN   TYPE      REASON                    OBJECT       MESSAGE
125m        Normal    RegisteredNode            node/5950x   Node 5950x event: Registered Node 5950x in Controller
122m        Normal    Starting                  node/5950x
122m        Normal    Starting                  node/5950x
121m        Normal    RegisteredNode            node/5950x   Node 5950x event: Registered Node 5950x in Controller
120m        Normal    Starting                  node/5950x
119m        Normal    RegisteredNode            node/5950x   Node 5950x event: Registered Node 5950x in Controller
81m         Normal    RegisteredNode            node/5950x   Node 5950x event: Registered Node 5950x in Controller
74m         Normal    RegisteredNode            node/5950x   Node 5950x event: Registered Node 5950x in Controller
4m39s       Normal    RegisteredNode            node/5950x   Node 5950x event: Registered Node 5950x in Controller
2m43s       Normal    RegisteredNode            node/5950x   Node 5950x event: Registered Node 5950x in Controller
3h24m       Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
3h24m       Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
3h24m       Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
3h24m       Normal    Starting                  node/pi0     Starting kubelet.
3h24m       Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
3h24m       Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
3h24m       Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
3h24m       Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
3h24m       Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
3h22m       Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
3h22m       Normal    Starting                  node/pi0
3h22m       Normal    Starting                  node/pi0
3h20m       Normal    Starting                  node/pi0
3h18m       Normal    Starting                  node/pi0
3h18m       Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
3h6m        Normal    Starting                  node/pi0     Starting kubelet.
3h6m        Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
3h6m        Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
3h6m        Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
3h6m        Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
3h6m        Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
3h5m        Normal    Starting                  node/pi0
3h5m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
3h4m        Normal    Starting                  node/pi0
3h2m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
3h2m        Normal    Starting                  node/pi0
3h          Normal    Starting                  node/pi0
179m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
176m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
175m        Normal    Starting                  node/pi0
170m        Normal    Starting                  node/pi0     Starting kubelet.
170m        Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
170m        Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
170m        Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
170m        Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
170m        Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
169m        Normal    Starting                  node/pi0
169m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
168m        Normal    Starting                  node/pi0
167m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
164m        Normal    Starting                  node/pi0
164m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
160m        Normal    Starting                  node/pi0
159m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
149m        Normal    Starting                  node/pi0     Starting kubelet.
149m        Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
149m        Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
149m        Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
149m        Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
149m        Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
148m        Normal    Starting                  node/pi0
148m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
146m        Normal    Starting                  node/pi0
146m        Normal    Starting                  node/pi0     Starting kubelet.
146m        Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
146m        Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
146m        Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
146m        Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
146m        Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
142m        Normal    Starting                  node/pi0
142m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
140m        Normal    Starting                  node/pi0
137m        Normal    Starting                  node/pi0
136m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
127m        Normal    Starting                  node/pi0     Starting kubelet.
127m        Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
127m        Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
127m        Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
127m        Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
127m        Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
126m        Normal    Starting                  node/pi0     Starting kubelet.
126m        Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
126m        Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
126m        Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
126m        Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
126m        Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
126m        Normal    Starting                  node/pi0
125m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
122m        Normal    Starting                  node/pi0     Starting kubelet.
122m        Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
122m        Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
122m        Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
122m        Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
122m        Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
122m        Normal    Starting                  node/pi0
121m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
120m        Normal    Starting                  node/pi0
119m        Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
117m        Normal    Starting                  node/pi0
106m        Normal    Starting                  node/pi0
82m         Normal    Starting                  node/pi0
81m         Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
75m         Normal    Starting                  node/pi0
74m         Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
5m48s       Normal    Starting                  node/pi0     Starting kubelet.
5m48s       Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
5m48s       Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
5m48s       Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
5m48s       Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
5m48s       Normal    NodeAllocatableEnforced   node/pi0     Updated Node Allocatable limit across pods
5m35s       Normal    Starting                  node/pi0
4m39s       Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
3m59s       Normal    Starting                  node/pi0
2m43s       Normal    RegisteredNode            node/pi0     Node pi0 event: Registered Node pi0 in Controller
2m11s       Normal    Starting                  node/pi0
70s         Normal    Starting                  node/pi0     Starting kubelet.
70s         Warning   InvalidDiskCapacity       node/pi0     invalid capacity 0 on image filesystem
70s         Normal    NodeHasSufficientMemory   node/pi0     Node pi0 status is now: NodeHasSufficientMemory
70s         Normal    NodeHasNoDiskPressure     node/pi0     Node pi0 status is now: NodeHasNoDiskPressure
70s         Normal    NodeHasSufficientPID      node/pi0     Node pi0 status is now: NodeHasSufficientPID
47s         Normal    Starting                  node/pi0
3h5m        Normal    NodeHasSufficientMemory   node/pi1     Node pi1 status is now: NodeHasSufficientMemory
3h5m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
3h4m        Normal    Starting                  node/pi1
3h4m        Normal    Starting                  node/pi1
3h3m        Normal    Starting                  node/pi1
3h2m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
3h1m        Normal    Starting                  node/pi1
179m        Normal    Starting                  node/pi1
179m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
176m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
176m        Normal    Starting                  node/pi1
169m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
167m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
164m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
159m        Normal    Starting                  node/pi1
159m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
153m        Normal    Starting                  node/pi1
148m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
147m        Normal    Starting                  node/pi1
145m        Normal    Starting                  node/pi1     Starting kubelet.
145m        Warning   InvalidDiskCapacity       node/pi1     invalid capacity 0 on image filesystem
143m        Normal    NodeHasSufficientMemory   node/pi1     Node pi1 status is now: NodeHasSufficientMemory
143m        Normal    NodeHasNoDiskPressure     node/pi1     Node pi1 status is now: NodeHasNoDiskPressure
143m        Normal    NodeHasSufficientPID      node/pi1     Node pi1 status is now: NodeHasSufficientPID
143m        Normal    NodeAllocatableEnforced   node/pi1     Updated Node Allocatable limit across pods
142m        Normal    Starting                  node/pi1
142m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
140m        Normal    Starting                  node/pi1
136m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
136m        Normal    Starting                  node/pi1
131m        Normal    Starting                  node/pi1
125m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
122m        Normal    Starting                  node/pi1
121m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
119m        Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
108m        Normal    Starting                  node/pi1
81m         Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
74m         Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
73m         Normal    Starting                  node/pi1
4m39s       Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
2m43s       Normal    RegisteredNode            node/pi1     Node pi1 event: Registered Node pi1 in Controller
121m        Normal    Starting                  node/pi2
121m        Normal    RegisteredNode            node/pi2     Node pi2 event: Registered Node pi2 in Controller
121m        Normal    Starting                  node/pi2
120m        Normal    Starting                  node/pi2
119m        Normal    RegisteredNode            node/pi2     Node pi2 event: Registered Node pi2 in Controller
117m        Normal    Starting                  node/pi2
108m        Normal    Starting                  node/pi2
81m         Normal    RegisteredNode            node/pi2     Node pi2 event: Registered Node pi2 in Controller
74m         Normal    RegisteredNode            node/pi2     Node pi2 event: Registered Node pi2 in Controller
4m39s       Normal    RegisteredNode            node/pi2     Node pi2 event: Registered Node pi2 in Controller
2m43s       Normal    RegisteredNode            node/pi2     Node pi2 event: Registered Node pi2 in Controller
```
```sh 
pi@pi0:~$ kubectl describe pod -n kube-system kube-apiserver-pi0
```
```yaml
Name:                 kube-apiserver-pi0
Namespace:            kube-system
Priority:             2000001000
Priority Class Name:  system-node-critical
Node:                 pi0/192.168.0.74
Start Time:           Sat, 31 Dec 2022 00:03:31 +0900
Labels:               component=kube-apiserver
                      tier=control-plane
Annotations:          kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: 192.168.0.74:6443
                      kubernetes.io/config.hash: 84ce72f8bb7e3c61ce2f90626840bead
                      kubernetes.io/config.mirror: 84ce72f8bb7e3c61ce2f90626840bead
                      kubernetes.io/config.seen: 2022-12-30T22:46:12.106958192+09:00
                      kubernetes.io/config.source: file
Status:               Running
IP:                   192.168.0.74
IPs:
  IP:           192.168.0.74
Controlled By:  Node/pi0
Containers:
  kube-apiserver:
    Container ID:  containerd://af41ac5e6c7660238b93a1ff23bf6a0fe137731754c6848b82e0ae7bafb56a35
    Image:         registry.k8s.io/kube-apiserver:v1.26.0
    Image ID:      registry.k8s.io/kube-apiserver@sha256:d230a0b88a3daf14e4cce03b906b992c8153f37da878677f434b1af8c4e8cc75
    Port:          <none>
    Host Port:     <none>
    Command:
      kube-apiserver
      --advertise-address=192.168.0.74
      --allow-privileged=true
      --authorization-mode=Node,RBAC
      --client-ca-file=/etc/kubernetes/pki/ca.crt
      --enable-admission-plugins=NodeRestriction
      --enable-bootstrap-token-auth=true
      --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
      --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
      --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
      --etcd-servers=https://127.0.0.1:2379
      --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt
      --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key
      --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
      --proxy-client-cert-file=/etc/kubernetes/pki/front-proxy-client.crt
      --proxy-client-key-file=/etc/kubernetes/pki/front-proxy-client.key
      --requestheader-allowed-names=front-proxy-client
      --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt
      --requestheader-extra-headers-prefix=X-Remote-Extra-
      --requestheader-group-headers=X-Remote-Group
      --requestheader-username-headers=X-Remote-User
      --secure-port=6443
      --service-account-issuer=https://kubernetes.default.svc.cluster.local
      --service-account-key-file=/etc/kubernetes/pki/sa.pub
      --service-account-signing-key-file=/etc/kubernetes/pki/sa.key
      --service-cluster-ip-range=10.96.0.0/12
      --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
      --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
    State:          Running
      Started:      Sat, 31 Dec 2022 02:10:14 +0900
    Last State:     Terminated
      Reason:       Error
      Exit Code:    137
      Started:      Sat, 31 Dec 2022 02:09:18 +0900
      Finished:     Sat, 31 Dec 2022 02:10:03 +0900
    Ready:          True
    Restart Count:  52
    Requests:
      cpu:        250m
    Liveness:     http-get https://192.168.0.74:6443/livez delay=10s timeout=15s period=10s #success=1 #failure=8
    Readiness:    http-get https://192.168.0.74:6443/readyz delay=0s timeout=15s period=1s #success=1 #failure=3
    Startup:      http-get https://192.168.0.74:6443/livez delay=10s timeout=15s period=10s #success=1 #failure=24
    Environment:  <none>
    Mounts:
      /etc/ca-certificates from etc-ca-certificates (ro)
      /etc/kubernetes/pki from k8s-certs (ro)
      /etc/pki from etc-pki (ro)
      /etc/ssl/certs from ca-certs (ro)
      /usr/local/share/ca-certificates from usr-local-share-ca-certificates (ro)
      /usr/share/ca-certificates from usr-share-ca-certificates (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  ca-certs:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/ssl/certs
    HostPathType:  DirectoryOrCreate
  etc-ca-certificates:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/ca-certificates
    HostPathType:  DirectoryOrCreate
  etc-pki:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/pki
    HostPathType:  DirectoryOrCreate
  k8s-certs:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/kubernetes/pki
    HostPathType:  DirectoryOrCreate
  usr-local-share-ca-certificates:
    Type:          HostPath (bare host directory volume)
    Path:          /usr/local/share/ca-certificates
    HostPathType:  DirectoryOrCreate
  usr-share-ca-certificates:
    Type:          HostPath (bare host directory volume)
    Path:          /usr/share/ca-certificates
    HostPathType:  DirectoryOrCreate
QoS Class:         Burstable
Node-Selectors:    <none>
Tolerations:       :NoExecute op=Exists
Events:
  Type     Reason          Age                     From     Message
  ----     ------          ----                    ----     -------
  Normal   SandboxChanged  3h23m                   kubelet  Pod sandbox changed, it will be killed and re-created.
  Normal   Pulled          3h23m                   kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   Created         3h23m                   kubelet  Created container kube-apiserver
  Normal   Started         3h23m                   kubelet  Started container kube-apiserver
  Normal   Killing         3h22m (x2 over 3h24m)   kubelet  Stopping container kube-apiserver
  Warning  Unhealthy       3h22m (x2 over 3h22m)   kubelet  Liveness probe failed: Get "https://192.168.0.74:6443/livez": dial tcp 192.168.0.74:6443: connect: connection refused
  Warning  Unhealthy       3h21m (x28 over 3h22m)  kubelet  Readiness probe failed: Get "https://192.168.0.74:6443/readyz": dial tcp 192.168.0.74:6443: connect: connection refused
  Normal   Pulled          3h6m                    kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   Created         3h6m                    kubelet  Created container kube-apiserver
  Normal   Started         3h6m                    kubelet  Started container kube-apiserver
  Warning  Unhealthy       3h5m (x3 over 3h6m)     kubelet  Startup probe failed: HTTP probe failed with statuscode: 500
  Warning  Unhealthy       3h4m (x17 over 3h4m)    kubelet  Readiness probe failed: Get "https://192.168.0.74:6443/readyz": dial tcp 192.168.0.74:6443: connect: connection refused
  Warning  Unhealthy       3h (x5 over 3h4m)       kubelet  Liveness probe failed: Get "https://192.168.0.74:6443/livez": dial tcp 192.168.0.74:6443: connect: connection refused
  Normal   Killing         175m (x4 over 3h4m)     kubelet  Stopping container kube-apiserver
  Normal   Pulled          170m                    kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   Created         170m                    kubelet  Created container kube-apiserver
  Normal   Started         170m                    kubelet  Started container kube-apiserver
  Normal   Killing         166m                    kubelet  Stopping container kube-apiserver
  Warning  Unhealthy       166m (x6 over 167m)     kubelet  Liveness probe failed: HTTP probe failed with statuscode: 500
  Warning  Unhealthy       166m (x16 over 167m)    kubelet  Readiness probe failed: HTTP probe failed with statuscode: 500
  Warning  BackOff         155m (x16 over 166m)    kubelet  Back-off restarting failed container kube-apiserver in pod kube-apiserver-pi0_kube-system(84ce72f8bb7e3c61ce2f90626840bead)
  Normal   Pulled          149m                    kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   Created         149m                    kubelet  Created container kube-apiserver
  Normal   Started         149m                    kubelet  Started container kube-apiserver
  Normal   SandboxChanged  146m                    kubelet  Pod sandbox changed, it will be killed and re-created.
  Normal   Pulled          146m                    kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   Created         146m                    kubelet  Created container kube-apiserver
  Normal   Started         146m                    kubelet  Started container kube-apiserver
  Warning  Unhealthy       143m (x2 over 146m)     kubelet  Liveness probe failed: Get "https://192.168.0.74:6443/livez": dial tcp 192.168.0.74:6443: connect: connection refused
  Warning  Unhealthy       143m (x18 over 146m)    kubelet  Readiness probe failed: Get "https://192.168.0.74:6443/readyz": dial tcp 192.168.0.74:6443: connect: connection refused
  Normal   Killing         133m (x3 over 146m)     kubelet  Stopping container kube-apiserver
  Normal   Pulled          127m                    kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   Created         127m                    kubelet  Created container kube-apiserver
  Normal   Started         127m                    kubelet  Started container kube-apiserver
  Normal   Pulled          123m                    kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   Created         123m                    kubelet  Created container kube-apiserver
  Normal   Started         123m                    kubelet  Started container kube-apiserver
  Warning  Unhealthy       122m                    kubelet  Startup probe failed: HTTP probe failed with statuscode: 500
  Normal   Killing         121m                    kubelet  Stopping container kube-apiserver
  Warning  Unhealthy       121m (x2 over 121m)     kubelet  Liveness probe failed: Get "https://192.168.0.74:6443/livez": dial tcp 192.168.0.74:6443: connect: connection refused
  Warning  Unhealthy       121m (x18 over 121m)    kubelet  Readiness probe failed: Get "https://192.168.0.74:6443/readyz": dial tcp 192.168.0.74:6443: connect: connection refused
  Warning  BackOff         97m (x80 over 121m)     kubelet  Back-off restarting failed container kube-apiserver in pod kube-apiserver-pi0_kube-system(84ce72f8bb7e3c61ce2f90626840bead)
  Normal   Pulled          6m8s                    kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   Created         6m8s                    kubelet  Created container kube-apiserver
  Warning  Unhealthy       5m30s (x3 over 5m50s)   kubelet  Startup probe failed: HTTP probe failed with statuscode: 500
  Warning  Unhealthy       3m37s (x8 over 3m59s)   kubelet  Readiness probe failed: HTTP probe failed with statuscode: 500
  Warning  Unhealthy       3m37s (x3 over 3m57s)   kubelet  Liveness probe failed: HTTP probe failed with statuscode: 500
  Normal   Killing         2m24s                   kubelet  Stopping container kube-apiserver
  Warning  Unhealthy       88s                     kubelet  Startup probe failed: Get "https://192.168.0.74:6443/livez": dial tcp 192.168.0.74:6443: connect: connection refused
  Normal   Pulled          83s                     kubelet  Container image "registry.k8s.io/kube-apiserver:v1.26.0" already present on machine
  Normal   SandboxChanged  83s                     kubelet  Pod sandbox changed, it will be killed and re-created.
  Normal   Created         83s                     kubelet  Created container kube-apiserver
  Normal   Started         83s                     kubelet  Started container kube-apiserver
  Normal   Killing         68s (x2 over 90s)       kubelet  Stopping container kube-apiserver
  Warning  Unhealthy       58s                     kubelet  Liveness probe failed: Get "https://192.168.0.74:6443/livez": dial tcp 192.168.0.74:6443: connect: connection refused
  Warning  Unhealthy       53s (x17 over 91s)      kubelet  Readiness probe failed: Get "https://192.168.0.74:6443/readyz": dial tcp 192.168.0.74:6443: connect: connection refused
```
  - `/etc/containerd/config.toml` 에서 `systemd_cgroup = true` 처리후 재시작 안되는 것으로 보임
    -> 해당 내용이 틀렸고 systemd_cgroup 이 아닌 SystemdCgroup 을 true 처리해야한다. 22.04 기준이다.

---
```sh
pi@pi1:~$ sudo kubeadm join [ip]:6443 --token [token]
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.
```
조인 후에 node 가 `Not ready` 상태

---
```sh 
$ systemctl status kubelet
● kubelet.service - kubelet: The Kubernetes Node Agent
     Loaded: loaded (/lib/systemd/system/kubelet.service; enabled; vendor preset: enabled)
    Drop-In: /etc/systemd/system/kubelet.service.d
             └─10-kubeadm.conf
     Active: activating (auto-restart) (Result: exit-code) since Sat 2022-12-31 04:31:33 UTC; 4s ago
       Docs: https://kubernetes.io/docs/home/
    Process: 4202 ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS (code=exited, status=1/FAILURE)
   Main PID: 4202 (code=exited, status=1/FAILURE)
        CPU: 102ms
       
$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --v=5
[...]
[preflight] Some fatal errors occurred:
        [ERROR CRI]: container runtime is not running: output: E1231 04:50:58.690702    9033 remote_runtime.go:948] "Status from runtime service failed" err="rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
time="2022-12-31T04:50:58Z" level=fatal msg="getting status of runtime: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
, error: exit status 1
```

```sh 
containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
sudo systemctl restart containerd
sudo sysctl --system
```
+ https://beer1.tistory.com/5

---
control-plane not ready
```sh 
$ kgno
NAME   STATUS     ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
pi0    NotReady   control-plane   12m   v1.26.0   192.168.0.74   <none>        Ubuntu 22.04.1 LTS   5.15.0-1012-raspi   containerd://1.6.14
```
cni 미 설치로 보이고 flannel 설치 후 해결되었다.

---
coredns, kube-proxy, kube-scheduler pending
```sh 
NAME                          READY   STATUS              RESTARTS         AGE
coredns-787d4945fb-87r4v      0/1     ContainerCreating   0                14m
coredns-787d4945fb-dmkxr      0/1     ContainerCreating   0                14m
etcd-pi0                      1/1     Running             82 (3m19s ago)   16m
kube-apiserver-pi0            1/1     Running             74 (5m53s ago)   15m
kube-controller-manager-pi0   1/1     Running             86 (6m24s ago)   15m
kube-proxy-knl9w              0/1     CrashLoopBackOff    8 (29s ago)      14m
kube-scheduler-pi0            0/1     CrashLoopBackOff    92 (24s ago)     16m
```
Pod sandbox changed, it will be killed and re-created.
- [ ] flannel 설치 이후 reboot, 그리고 kubelet 이 뜨지 않아서 강제 재시작을 함

---
```sh 
Unable to connect to the server: x509: certificate is valid for kubernetes, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc.cluster.local, [local master node name], not [external.cluster.domain]
```
```sh
sudo rm /etc/kubernetes/pki/apiserver.*
kubeadm init phase certs all --apiserver-advertise-address=0.0.0.0 --apiserver-cert-extra-sans=[master node ip,domain name]
```
master node internal ip, 그리고 외부에서 접속하고자하는 도메인 명을 `,` 를 통해 함께 기재해야지 내부 외부 모두에서 사용이 가능
+ **v** https://stackoverflow.com/questions/46360361/invalid-x509-certificate-for-kubernetes-master
```sh 
kubectl get configmap kubeadm-config -n kube-system
```
  - [ ] 해당 내용에 따르면 내용과 별 관련이 없다 해당 내용은 저장을 위한 것인지 아니면 혹시 renew 단계에서 에러를 일으키는 것인지 renew 로 테스트가 필요하다
+ https://blog.dudaji.com/kubernetes/2020/04/08/add-ip-to-kube-api-cert.html
  - 해당 링크에는 `kubeadm-config` 를 함께 수정한다
  
## reboot|재부팅 
재부팅 후에는 kubectl 을 통해서 접근이 되지 않는다.
```sh 
systemctl status kubelet
```
을 보면 active 상태가 아닌 것을 확인할 수 있다  
단순히 아래 커맨드로 복구가 가능하다
```sh
sudo sysctl --system
```
- 2023-02-02 update
  복구가 되지 않았고 `kubeadm init` 을 다시 해보았으나 swap 파티션을 이유로 뜨지 않았다
  swap 파티션을 내리고 init 을 해도 되었을 것으로 생각되나 swap 파티션이 부팅시 뜨지 않게 한 후 reboot 을 하면 kubelet 이 정상적으로 부팅시에 시작된다
다만 영구적 설정이 되지 않는 경우에는 해당 커맨드를 재 실행해줘야한다

예를 들어 /etc/fstab 에 swap mount 가 있어서 재부팅마다 swap 이 살아난다면 아래와 같이 처리해야한다
```sh 
$ cat /proc/swaps
# 결과가 있는 경우
$ sudo swapoff -a
```
- [X] DONE: 2023-02-02 자동화
  - /etc/fstab 을 수정해서 swap 파티션이 뜨지 않도록 설정하면 리붓후 알아서 kubelet 이 뜬 것을 확인할 수 있다.

## link
- [[raspberry-pi]]
- [[kubelet]]
- [[crictl]]
- [[systemctl]]
