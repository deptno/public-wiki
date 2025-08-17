# vpn | virtual private network
> 가상으로 특정 네트워크에 속하도록 만든다

## 활용
- block, ban 등을 우회하는 용도로 사용
- 특정 국가 인증을 위해 사용

## [[kubernetes]]
- proton vpn 을 기준으로 linux 설정을 제공한다
- terminal 만 존재하는 서버 환경에서도 사용할 수 있도록 `config` 를 제공
  - [ ] [[openvpn]]
  - [X] [[wireguard]]
    - 간편하다고 해서 픽
- 파일 다운로드 후 [[pod]] 에 주입하기 위해서는 `/etc/wireguard/[config_name.conf]` 주입(마운트)이 필요
  - 이번 케이스는 [[configmap]] 을 통해서 주입
- 를 활성화해서 만들어진 config 를 적용할 필요있음, `.conf` 제외하고 입력
```sh 
  `wg up [config_name]` 
```
- `Dockerfile` 단계에서 `ENTRYPOINT` 를 통해서 스크립트로 활성 화 후 `CMD` 진입
```sh 
#!/bin/sh
# entorypoint 에 주입될 스크립트
wg-quick up [config_name]

exec "$@"
```


### + 로컬 환경 트래픽 지원
> 로컬 환경에도 접근을 해야하는 경우, eg, nas, db

- 이런 경우에는 추가 설정이 필요
  + https://www.procustodibus.com/blog/2021/03/wireguard-allowedips-calculator/
    - 정독이 가장 빠른길
- `ip route` 에 로컬 서브넷들을 추가하여 `vpn` 으로 트래픽이 가지 않도록 제외 시켜줘야 함
- 이를 위해서는 [[kubernetes]] cluster 가 사용하는 subnet 을 알 필요가 있음
  - `kube-system` namespace 의 `kubeadm-config` [[configmap]] 참조
    - `podSubnet`, `serviceSubnet` 확인
- `etc/wireguard/[config_name.conf]` 에 마운트 되는 파일에 아래 내용이 필요

```sh 
[Interface]
...
# DNS [VPN_DNS_IP]
PreUp = ip route add [KUBERNETES_SERVICE_SUBNET] dev eth0
PreUp = ip route add [KUBERNETES_POD_SUBNET] dev eth0
PostUp = echo 'nameserver [VPN_DNS_IP]' >> /etc/resolve.conf
PostDown = ip route del [KUBERNETES_SUBNET] dev eth0
PostDown = ip route del [KUBERNETES_POD_SUBNET] dev eth0
```
  - `DNS ....` 줄은 주석 처리한다. 안하는 경우 `/etc/resolve.conf` 덮어 써짐.
  - 위에 내용이 추가됨으로 써 local network traffic 은 따로 라우팅

## service
- proton vpn
  - 무료로 제공

## link
- [[ip]]
- [[kubernetes]]
- [[pod]]
- [[protonvpn]]
