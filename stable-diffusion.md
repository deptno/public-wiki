# stable-diffusion

## 실행
1. 클론
2. 실행 `./webui.sh`

### 옵션
- 커맨드라인 옵션
  + https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Command-Line-Arguments-and-Settings
  - 해당 설정 적용은 `webui-user.sh` 에서 덮어쓰게되어있으니 해당 파일을 참조해서 처리해야한다

###  모델 위치 변경등
- `webui-user.sh` 참고
  - `COMMANDLINE_ARGS=--data-dir [절대경로]` 내용 포함, 상대경로 처리안됨
 

## [[error]]
### Cannot locate TCMalloc
- [[ubuntu]] `./webui.sh` 실행시 에러
```sh 
Cannot locate TCMalloc. Do you have tcmalloc or google-perftool installed on your system? (improves CPU memory usage)
```
  - 패키지 설치
  ```sh 
  sudo apt install google-perftools
  ```
  + https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/10117#issuecomment-1536437860

### RuntimeError: Torch is not able to use GPU; add --skip-torch-cuda-test to COMMANDLINE_ARGS variable to disable this check
- [[cuda]] 참조

### remote 서버로 사용시 ip 로 접속이 안됨
- [[ubuntu]] 인 경우
```sh
sudo service ufw stop # 방화벽 제거
./webui.sh --listen
```
  - `--listen` 옵션을 줘야지 로컬외 접속이 허용된다
  + https://www.reddit.com/r/StableDiffusion/comments/15ge386/comment/jui85sv/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

## link
- [[ai]]
- [[midjourney]]
- [[comfyui]]
- [[python]]
- [[book/making-game-graphic-with-stable-diffusion]]
- [[huggingface-ml-for-games]]
