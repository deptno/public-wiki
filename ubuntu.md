# ubuntu

## 22.04 server lts  설치 기록
- iso -> usb -> 기본설정 설치
- github 에서 pub 읽어올 수 있음
- 부팅 후
```sh
sudo apt update
sudo apt upgrade
sudo apt install net-tools # ifconfig
pip install pipenv # 아래 패스추가해야한다
echo 'export PATH=~/.local/bin:$PATH' >> ~/.bashrc
sudo service ufw stop # 외부에서 접속하기위해 방화벽 비활성화
```
- `python3`, `tmux` 설치되어있음

## link
- [[kubernetes]]
- [[ai]]
- [[stable-diffusion]]
- [[python]]
- [[pipenv]]
