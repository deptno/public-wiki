# kubernetes|쿠버네티스

## pod
- init container
  설정을 위해 선실행되고 종료되는 컨테이너
  + https://kubernetes.io/ko/docs/concepts/workloads/pods/init-containers/
## secret
echo 를 사용하면 newline `\n` 이 붙게된다.
- `echo -n` 을 사용
- `- tr -d '\n'`
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
