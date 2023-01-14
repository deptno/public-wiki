# raspberry pi

## [[kubernetes]] setup
[[kubespray]] 로 시간 많이 날림
[[kubeadm]] 으로 성공 ha 를 구성하려고 k3s 대신 k8s 를 선택했으나 api-server 자체가 load balancer 를 필요로 하기 때문에 그냥 단독으로 사용

### kubeadm

os: Ubuntu Server 22.04.1 LTS (64-bit) - Server OS with long-term support for RPi

- 공통
```sh
cgroup="$(head -n1 /boot/firmware/cmdline.txt) cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1 swapaccount=1"
echo $cgroup | sudo tee /boot/firmware/cmdline.txt

sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

# off swap
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
sudo swapoff -a

# containerd
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=arm64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update
sudo apt install -y containerd.io

sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml

# 안되는 경우
sudo rm /etc/containerd/config.toml
sudo systemctl restart containerd

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
```
- master node (control plane)
```sh
sudo kubeadm config images pull
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 
```
`init` 의 결과로 나오는 스크립트 실행해서 ~/.kube/config 를 생성하면 [[kubectl]] 사용가능
- worker node
`init` 의 결과로 아래 형식의 스크립트를 복붙하여 실행
```sh 
sudo kubeadm join 192.168.0.74:6443 --token [token] [--node-name name_default:hostname]
```
- x86 의 경우 containerd 가 설치되지 않으므로 # containerd 부분을 아래로 대신한다

```sh 
# x86
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
#### error
[[kubeadm]] 참조

## related
- [[kubeadm]]
