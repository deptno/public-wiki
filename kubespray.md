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

> 실패시
```sh
ssh-keygen
cat ~/.ssh/id_rsa.pub # 복사
vi ~/.ssh/authorized_keys # 마지막 줄에 추가, /etc/hosts 에 설정한 컨트롤 플레인이 될 모든 노드에 대해 설정
ssh [name - /etc/hosts 에 기재한 이름] # 접속 확인
```

### configure kubespray
```sh
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
pip3 install -r requirements.txt
cp -r inventory/sample inventory/mycluster
vi inventory/mycluster/hosts.yml # 설정은 kubespray 문서 참고
vi inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml # 설정은 kubespray 문서 참고
ansible-playbook -i inventory/mycluster/hosts.yml --become --become-user=root -v cluster.yml
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

#### error
```sh
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```
`ansible-playbook` 명령어를 실행할때 `--become-user=root` 가 되어 있어 `~/.kube/config` 가 `root` 를 기준으로 생성된 것으로 생각된다.

## error
```
fatal: [kube02]: FAILED! => {"changed": false, "msg": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic\n", "name": "nf_conntrack_ipv4", "params": "", "rc": 1, "state": "present", "stderr": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic\n", "stderr_lines": ["modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/5.15.0-53-generic"], "stdout": "", "stdout_lines": []}
...ignoring
Sunday 27 November 2022  17:37:38 +0900 (0:00:00.273)       0:11:17.710 *******

TASK [kubernetes/node : Persist ip_vs modules] **************************************************************************************************************
changed: [kube00] => {"changed": true, "checksum": "963dae5d0f149158bd6b9a750827856f6f2382fd", "dest": "/etc/modules-load.d/kube_proxy-ipvs.conf", "gid": 0, "group": "root", "md5sum": "0f7f7753a47d8c043fb1f8043b65beb4", "mode": "0644"
```
`ansible-playbook` 커맨드 중에 보인 에러 메시지인데 무시해도 잘되었다.

## related
- [[multipass]]
- [[kubectl]]
