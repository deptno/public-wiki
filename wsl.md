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

## error
- wsl 실행하니 창 열리고 바로 꺼짐
- 지우고 설치하니 default profile, guid 어쩌고하고 꺼짐
- amd cpu 를 사용하고 있는데 bios 에서 svm 이 활성화가 꺼져있어서 생긴 문제로 다시 켜주면 실행됨

## link
- [[windows]]
