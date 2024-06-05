# stable-diffusion-recipe
## system
- cpu: 5600x
- ram: 32G
- vga: rtx3080/10G

## recipe
### 영상에서 캐릭터만 변경
> cpu, ram, gpu, vram 모두 풀로 사용된다, streaming 방식을 코드 구현을 고민해봐야할듯
> 160x412 영상: 854sec/32fps

#### model
- Load checkpoint -> AnimateDiff Loader
- 
#### clip(positive)
- load video -> image -> controlnet: (openpose -> depth -> lineart)
- Load checkpoint -> CLIP Text encode -> controlnet: (openpose -> depth -> lineart)

#### latent
- load video -> image -> rvm 을 통한 배경 제거 마스크 -> VAE Encode (for Inpainting)
- load video -> image -> pixel -> VAE Encode (for Inpainting)
- Load checkpoint -> vae -> VAE Encode (for Inpainting)

## link
- [[stable-diffusion]]
- [[comfyui]]
