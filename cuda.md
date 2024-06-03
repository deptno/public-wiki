# cuda

## 설치
```sh 
nvidia-smi
```
- 명령어가 있는지 확인
- 없는 경우 설치 진행
+ https://developer.nvidia.com/cuda-downloads
- 사용 시스템과 아키텍쳐 os 를 고른후 *network* 선택하여 스크립트 실행

## error
### RuntimeError: Torch is not able to use GPU; add --skip-torch-cuda-test to COMMANDLINE_ARGS variable to disable this check
```sh 
RuntimeError: Torch is not able to use GPU; add --skip-torch-cuda-test to COMMANDLINE_ARGS variable to disable this check
```
- [[ubuntu]] 에서 [[stable-diffusion]] 실행시 발생
- 해결
```sh 
$ pip install torch torchvision torchaudio
```
+ https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_network
- 설치후
```sh 
$ nvidia-smi
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.
```
- 이후에도 동일하게 에러발생
```sh 
$ sudo apt-get install cuda-toolkit
```
- 설치해도 동일하게 에러 발생 `reboot` 후 gpu 인식이 시작됐는데 `coda-toolkit` 때문인지는 확인 필요
- **정리**
  - `torch` 설치는 `./webui.sh` 에 ㅍ함되어있지 않을까 예상
  - 결국 nvidia 드라이버 이슈
  - 위 링크를 통해서 cuda-toolkit, cuda-driver 이슈가 아닐까함 다시 시도시 재현해서 정리
    + https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_network
    + https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#ubuntu

## link
- [[nvidia]]
- [[stable-diffusion]]
- [[comfyui]]
- [[ai]]
