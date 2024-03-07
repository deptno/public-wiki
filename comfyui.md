# comfyui
[[stable-diffusion]] ui

## 설치
```sh 
# 파이토치 for apple sillicon
# + https://developer.apple.com/metal/pytorch/
pip3 install --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/cpu
```

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
