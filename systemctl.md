# systemctl

init.d 제어

```sh
sudo systemctl status [service-name]
sudo systemctl enable [service-name] # 부팅시 시작
sudo systemctl disable [service-name] # 부팅시 시작하지 않도록
sudo systemctl start [service-name]
sudo systemctl stop [service-name]
sudo systemctl restart [service-name] # stop -> start
```

## releated
- [[linux]]
- [[kubeadm]]
