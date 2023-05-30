# kubespray

## setup
3대 이상의 vm([[multipass]] 참고) 준비
[[multipass]] 사용을 가정하기 때문에 ubuntu 를 기준으로 작성됨

### dns
```sh
sudo vi /etc/hosts
# 3대에 접근할 수 있도록 정보 설정
```

### configure access permission
```sh
ssh-keygen
ssh-copy-id [name - /etc/hosts 에 기재한 이름]
```

**자기 자신도 ansible 대상이라 함께 복사해야한다**

> 실패시
```sh
ssh-keygen
cat ~/.ssh/id_rsa.pub # 복사
vi ~/.ssh/authorized_keys # 마지막 줄에 추가, /etc/hosts 에 설정한 컨트롤 플레인이 될 모든 노드에 대해 설정
ssh [name - /etc/hosts 에 기재한 이름] # 접속 확인
```

### configure kubespray
```sh
git clone -b v2.22.0 https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
sudo apt install python3-pip -y
sudo pip3 install -r requirements.txt
cp -r inventory/sample inventory/pi
vi inventory/pi/hosts.yml # 설정은 kubespray 문서 참고
vi inventory/pi/group_vars/k8s_cluster/k8s-cluster.yml # 설정은 kubespray 문서 참고
ansible-playbook -i inventory/pi/hosts.yml --become --become-user=root -v cluster.yml
```

```inventory/pi/hosts.yml
all:
  hosts:
    pi0:
      ansible_host: pi0
    pi1:
      ansible_host: pi1
    pi2:
      ansible_host: pi2
  children:
    kube_control_plane:
      hosts:
        pi0:
        pi1:
        pi2:
    kube_node:
      hosts:
        pi0:
        pi1:
        pi2:
    etcd:
      hosts:
        pi0:
        pi1:
        pi2:
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
        calico_rr:
    calico_rr:
      hosts: {}
```
```k8s-cluster.yml
kube_proxy_strlct_arp: true # MetalLB 설정
kubernetes audit: true
```

### install kubectl
```sh
sudo snap install kubectl --classic
kubectl get nodes -o wide
```

### configure local kube/config
여기는 localhost 기 때문에 물리적인 사용 컴퓨터, 나의 경우에는 mac 이다.
`kubectl` 이 깔려있다고 가정한다.

```sh
vi ~/.kube/config # vm 내의 `/root/.kube/config` 를 복사해온다, 모르겟으면 [[multipass]] mount 참조
# clusters.cluster[].server 의 ip control-plane 의 ip로 교체한다.
# 2022-11-27 책에서는 추가으로 아래 정보 수정을 하고 있는데 이건 여러 클러스터를 사용할때 로컬에서 구분하기 위한 거으로 생각된다.
# - clusters.cluster[].name
# - clusters.contexts[].context[].cluster
# - clusters.contexts[].context[].user
# - clusters.contexts[].name
# - current-context
# - users[].name
```
```sh 
$ kubectl get nodes
NAME     STATUS   ROLES           AGE   VERSION
kube00   Ready    control-plane   31m   v1.25.4
kube01   Ready    control-plane   30m   v1.25.4
kube02   Ready    control-plane   30m   v1.25.4
```

### error
```sh
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```
`ansible-playbook` 명령어를 실행할때 `--become-user=root` 가 되어 있어 `~/.kube/config` 가 `root` 를 기준으로 생성된 것으로 생각된다.

---
```sh
fatal: [kube02]: FAILED! => {"changed": false, "msg": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic\n", "name": "nf_conntrack_ipv4", "params": "", "rc": 1, "state": "present", "stderr": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic\n", "stderr_lines": ["modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic"], "stdout": "", "stdout_lines": []}
```
```sh
fatal: [kube02]: FAILED! => {"changed": false, "msg": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic\n", "name": "nf_conntrack_ipv4", "params": "", "rc": 1, "state": "present", "stderr": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic\n", "stderr_lines": ["modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic"], "stdout": "", "stdout_lines": []}
...ignoring
Sunday 27 November 2022  17:37:38 +0900 (0:00:00.273)       0:11:17.710 *******

TASK [kubernetes/node : Persist ip_vs modules] **************************************************************************************************************
changed: [kube00] => {"changed": true, "checksum": "963dae5d0f149158bd6b9a750827856f6f2382fd", "dest": "/etc/modules-load.d/kube_proxy-ipvs.conf", "gid": 0, "group": "root", "md5sum": "0f7f7753a47d8c043fb1f8043b65beb4", "mode": "0644"
```
`ansible-playbook` 커맨드 중에 보인 에러 메시지인데 무시해도 잘되었다.
---
```sh
fatal: [kube03]: FAILED! => {"msg": "Missing sudo password"}
```
```sh
ubuntu@kube00:~/kubespray$ ansible-playbook -i inventory/mycluster/hosts-new02.yml -b facts.yml
[WARNING]: Skipping callback plugin 'ara_default', unable to load

PLAY [Gather facts] *************************************************************
Thursday 22 December 2022  13:40:19 +0900 (0:00:00.012)       0:00:00.012 *****

TASK [Gather minimal facts] *****************************************************
ok: [kube00]
ok: [kube02]
ok: [kube01]
fatal: [kube03]: FAILED! => {"msg": "Missing sudo password"}
```

`sudo -l` 커맨트를 통해 나오는 순서가 의미가 있으며 오버라이딩 되어 다르게 동작 할 수 있다.  
- https://github.com/ansible/ansible/issues/71939#issuecomment-702185274
유저에 대한 권한을 맨 아랫줄로 옮겨서 주니 해결됨 과 같은 상태가 되었으며 [[kubespray]] 가 정상 동작했다.

```/etc/sudoers
# User privilege specification
root    ALL=(ALL:ALL) ALL
# ! 문제 영역
# ubuntu  ALL=(ALL) NOPASSWD: ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "@include" directives:
# +update: 2022-12-29 ! 이동 후 해결된 영역
ubuntu  ALL=(ALL) NOPASSWD: ALL
# -update: 2022-12-29

@includedir /etc/sudoers.d
# ! 이동 후 해결된 영역
ubuntu  ALL=(ALL) NOPASSWD: ALL
```
### 문제상황
```sh 
ubuntu@kube03:~$ sudo -l
Matching Defaults entries for ubuntu on kube03:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User ubuntu may run the following commands on kube03:
    (ALL) NOPASSWD: ALL
    (ALL : ALL) ALL
ubuntu@kube03:~$ !v
vi /etc/sudoers
ubuntu@kube03:~$ sudo vi /etc/sudoers
```
### 해결됨
```sh
ubuntu@kube03:~$ sudo -l
Matching Defaults entries for ubuntu on kube03:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User ubuntu may run the following commands on kube03:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: ALL
ubuntu@kube03:~$
```
---
```sh
fatal: [kube03]: FAILED! => {"changed": false, "elapsed": 300, "msg": "Timeout when waiting for file /etc/cni/net.d/calico-kubeconfig"}
```
---
```sh 
fatal: [pi0]: FAILED! => {"changed": false, "msg": "modprobe: FATAL: Module dummy not found in directory /lib/modules/5.15.0-1012-raspi\n", "name": "dummy", "params": "numdummies=0", "rc": 1, "state": "present", "stderr": "modprobe: FATAL: Module dummy not found in directory /lib/modules/5.15.0-1012-raspi\n", "stderr_lines": ["modprobe: FATAL: Module dummy not found in directory /lib/modules/5.15.0-1012-raspi"], "stdout": "", "stdout_lines": []}
```
- 답을 찾지 못한 상태, ubuntu 22.04 -> ubuntu 20.04 로 다운그레이드함
---
```sh
Failed to connect to the host via ssh: ssh: Could not resolve hostname pi2: Temporary failure in name resolution"
```
/etc/hosts 나 dhcp 서버서 resolv가 안되는 것으로 보인다

---
```sh 
the output has been hidden due to the fact that 'no_log: true' was specified for this result"
```
설치시에 노드 중 다른 아키텍쳐가 있으면 발생하는 것으로 생각됨, eg) control-plane (arm) + worker node (x86) 조합

---
```sh 
TASK [etcd : Configure | Wait for etcd cluster to be healthy] ***********************************************************************************************************************************************************************
task path: /Users/deptno/workspace/src/github.com/kubespray/roles/etcd/tasks/configure.yml:82
fatal: [pi0]: FAILED! => {
    "attempts": 4,
    "changed": false,
    "cmd": "set -o pipefail && /usr/local/bin/etcdctl endpoint --cluster status && /usr/local/bin/etcdctl endpoint --cluster health 2>&1 | grep -v 'Error: unhealthy cluster' >/dev/null",
    "delta": "0:00:05.194729",
    "end": "2022-12-30 11:54:44.046275",
    "invocation": {
        "module_args": {
            "_raw_params": "set -o pipefail && /usr/local/bin/etcdctl endpoint --cluster status && /usr/local/bin/etcdctl endpoint --cluster health 2>&1 | grep -v 'Error: unhealthy cluster' >/dev/null",
            "_uses_shell": true,
            "argv": null,
            "chdir": null,
            "creates": null,
            "executable": "/bin/bash",
            "removes": null,
            "stdin": null,
            "stdin_add_newline": true,
            "strip_empty_ends": true,
            "warn": false
        }
    },
    "msg": "non-zero return code",
    "rc": 1,
    "start": "2022-12-30 11:54:38.851546",
    "stderr": "{\"level\":\"warn\",\"ts\":\"2022-12-30T11:54:43.954+0900\",\"logger\":\"etcd-client\",\"caller\":\"v3@v3.5.6/retry_interceptor.go:62\",\"msg\":\"retrying of unary invoker failed\",\"target\":\"etcd-endpoints://0x40000d4c40/192.168.0.74:2379\",\"attempt\":0,\"error\":\"rpc error: code = DeadlineExceeded desc = context deadline exceeded\"}\nFailed to get the status of endpoint https://192.168.0.78:2379 (context deadline exceeded)",
    "stderr_lines": [
        "{\"level\":\"warn\",\"ts\":\"2022-12-30T11:54:43.954+0900\",\"logger\":\"etcd-client\",\"caller\":\"v3@v3.5.6/retry_interceptor.go:62\",\"msg\":\"retrying of unary invoker failed\",\"target\":\"etcd-endpoints://0x40000d4c40/192.168.0.74:2379\",\"attempt\":0,\"error\":\"rpc error: code = DeadlineExceeded desc = context deadline exceeded\"}",
        "Failed to get the status of endpoint https://192.168.0.78:2379 (context deadline exceeded)"
    ],
    "stdout": "https://192.168.0.77:2379, afd0f05f33356a3a, 3.5.6, 20 kB, false, false, 9, 30, 30, \nhttps://192.168.0.74:2379, f0a124daf3245703, 3.5.6, 20 kB, true, false, 9, 30, 30, ",
    "stdout_lines": [
        "https://192.168.0.77:2379, afd0f05f33356a3a, 3.5.6, 20 kB, false, false, 9, 30, 30, ",
        "https://192.168.0.74:2379, f0a124daf3245703, 3.5.6, 20 kB, true, false, 9, 30, 30, "
    ]
}

```
```sh 
sudo service ufw stop
sudo vi /etc/hosts
```
- [X] 방화벽 해제
- [X] `sudo vi /etc/hosts` 호스트간 명시 이후 에러메시지 변함 -> 재시작 하니 같음
```sh 
fatal: [pi0]: FAILED! => {
    "attempts": 5,
    "changed": false,
    "cmd": [
        "/usr/local/bin/kubeadm",
        "--kubeconfig",
        "/etc/kubernetes/admin.conf",
        "token",
        "create"
    ],
    "delta": "0:01:15.145403",
    "end": "2022-12-30 12:25:47.954213",
    "invocation": {
        "module_args": {
            "_raw_hello_exchange: master version 4\r\ndebug3: mux_client_forwards: request forwardings: 0 local, 0 remote\r\ndebug3: mux_client_request_session: entering\r\ndebug3: mux_client_request_alive: entering\r\ndebug3: mux_client_request_alive: done pid = 82745\r\ndebug3: mux_client_request_session: session request sent\r\ndebug1: mux_client_request_session: master session id: 4\r\ndebug3: mux_client_read_packet: read header failed: Broken pipe\r\ndebug2: Received exit status from master 1\r\n")
```
- [X] `declare IPS` 로 control-plane ip 설정 -> 실패
- [ ] `ansible-playbook [....] --private-key=~/.ssh/[private key]
- [ ] 해결 안됨
## related
- [[multipass]]
- [[kubectl]]
- [[sudo]]
- [[book/24-steps-k8s]]
