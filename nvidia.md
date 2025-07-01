# nvidia

## kubernetes 에 추가하기
- [[kubernetes]] node
```sh
sudo apt install nvidia-headless-570 nvidia-utils-570 # 570 은 내가 설치 당시에 번호, 최신으로 설치함
sudo reboot # 재부팅 필요
nvidia-smi # 사용가능 테스트
```
- [[helm]] 으로 `nvidia-device-pluign` 설치 필요, 기본 설정으로 가능, 현재기준 `0.17.0` 버전
- gpu 를 가진 노드에 annotation 추가
  - `nvidia.com/presernt: "true"`
  - 설정 이후에 [[daemonset]] 을 통해 [[pod]] 뜨는 것 확인 가능

### 노드 추가부터 진행시
- swapoff
- containerd 설치
- apt-get kubernetes 저장소 추가
- kubeadm, kubelet, kubectl 설치, 마스터 노드 버전에 맞춰서
- join master node
- nvidia-container-tookit 설치
- nvidia-headless-570 nvidia-utils-570, nvidia-driver-570-server
- nvidia-smi 확인

## link
- [[cuda]]
