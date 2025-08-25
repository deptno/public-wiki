# netplan
> `/etc/netplan/*` 파일 설정과 관련이 있고 고정 아이피 설정등에 사용된다

## ip bonding
- 여러개의 ethernet 을 가지고 하나로 묶는 기술
- 여러가지 형태로 묶을 수 있는데 여기서는 `active-backup` 으로 진행
  - 두 선중에 하나가 백업으로 대기후 문제가 생기면 사용되는 방식
- `vi /etc/netplan/00-installer-config.yaml`
### before
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp42s0:
      dhcp4: true
    enp6s0:
      dhcp4: true
  version: 2
```

### after
```
# This is the network config written by 'subiquity'
network:
  ethernets:
# 2.5G NIC을 기본으로
    enp42s0: {}
    enp6s0: {}
  version: 2
  bonds:
    bond0:
      macaddress: aa:bb:cc:dd:ee:ff
      interfaces:
        - enp42s0
        - enp6s0
      parameters:
        mode: active-backup
        primary: enp42s0
        mii-monitor-interval: 100   # 100ms 간격으로 링크 감시
      dhcp4: true
```
- 이후 로딩 확인
```
# 본딩 모듈 로드 여부 확인
lsmod | grep bonding
# 없으면 로드
sudo modprobe bonding
# 영구 추가
echo "bonding" | sudo tee -a /etc/modules

sudo netplan --debug generate
sudo netplan --debug apply

# mii-monitor-interval: 100
sudo reboot

ethtool bond0 | grep -i speed
```
- `mii-monitor-interval` 은 버그인지 `reboot` 을 하지 않으면 적용되지 않음

## link
- [[terminal]]
- [[ip]]
- [[ifconfig]]
