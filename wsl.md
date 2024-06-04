# wsl
> windows subsystem for linux

## 설치
- store 에서 wsl 설치
- os 설치, wsl 실행(터미널)
```sh
wsl --list --online # distro 목록
wsl --install [DISTRO]
```
- os 설치(ubuntu 22.04 lts) 후, wsl 실행해서 subsystem shell 열기
- [옵션] [[ai]] 활용을 위한 [[pipenv]] 설치
```sh 
apt update
apt install python3-pip
pip install pipenv
```

## 설정
- 하드웨어 리소스 설정이나 터널링등에 대한 설정이 `.wslconfig` 파일을 통해 가능하다
- 탐색기에서 `%userprofile%` 주소로 이동
- `.wslconfig` 생성
- 기본값이나 설정할 수 있는 값을 확인하기 위해서는 관련 문서 참조 필요
- CPU, memory 등을 통해서 하드웨어 스펙 전달 가능, **재부팅** 필요

## 외부에서 ssh 를 통해 접속

### window host 설정
1. window에서 openssh server 설치 및 시작
2. 관리자 계정으로 power shell 오픈
3. `net user [ID] [PASSWORD] /add` 명령으로 사용자 생성, 패스워드 필요
4. `ssh [WINDOWS_HOST_IP]` 접속

### wsl os 설정
1. `openssh-server` 설치
```sh 
apt update
apt upgrade
apt install openssh-server
```

### remote -> window host 
> `net user` 를 통해 생성한 계정으로 접속
```sh 
ssh [USER]@[WINDOWS_HOST_IP]
```

### remote -> window host -> wsl

## error
- wsl 실행하니 창 열리고 바로 꺼짐
- 지우고 설치하니 default profile, guid 어쩌고하고 꺼짐
- amd cpu 를 사용하고 있는데 bios 에서 svm 이 활성화가 꺼져있어서 생긴 문제로 다시 켜주면 실행됨

## link
- [[windows]]
- [[ssh]]
