# comfyui
[[stable-diffusion]] ui

## 설치
```sh 
# 파이토치 for apple sillicon
# + https://developer.apple.com/metal/pytorch/
$ pip3 install --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/cpu

# 가상환경 구성, 혹시나 해서 한번 해봄
$ pipenv shell

# 디펜턴시 설치
$ pip install -r requirements.txt

# 실행
$ python main.py

Total VRAM 65536 MB, total RAM 65536 MB
Set vram state to: SHARED
Device: mps
VAE dtype: torch.float32
Using sub quadratic optimization for cross attention, if you have memory or speed issues try using: --use-split-cross-attention
****** User settings have been changed to be stored on the server instead of browser storage. ******
****** For multi-user setups add the --multi-user CLI argument to enable multiple user profiles. ******
Starting server

To see the GUI go to: http://127.0.0.1:8188
```
- 이후 주소 접속시 ui 확인 됨

## 개념
### checkpoint
#### clip
- prompt(string) -> model 이 이해하는 형태
- text encoder 개념

#### model
- denoise 주체

#### vae
- latent image - model 이 이해하는 이미지
- pixel image - 우리가 보는 이미지
- latent image(from sampler) -> pixel image
- decoder

## link
- [[stable-diffusion]]
