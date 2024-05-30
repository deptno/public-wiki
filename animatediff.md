# animatediff

- [ ] TemporalDiff 라는 것도 있음
  + https://huggingface.co/CiaraRowles/TemporalDiff

## setup
+ https://github.com/Kosinkadink/ComfyUI-AnimateDiff-Evolved
- motion module 이 필요
  - module 마다 안정화 정도와 파인튜닝 내용이 다름
- `/custom_nodes/` 와 `models/`, 두개중 하나로 설정을 저장가능
  - `/custom_nodes/` 으로 시작
- `/custom_nodes/ComfyUI-AnimateDiff-Evolved/models/` motion 파일
- `/custom_nodes/ComfyUI-AnimateDiff-Evolved/motion_lora/`, **v2-based motion** 에 영향을 주고 싶다면 로라 사용가능

## [[comfyui]]
### 연결
- 추가되는 노드
  - `Animatied Diff Loader` -> checkpoint 와 페어링된, motion model 을 선택, 
  - `Video Combine VHS` -> 영상생성
- `Load Checkpoint` -> `Animatied Diff Loader`
- `VAE Decoder` -> `Video Combine VHS`
- `Empty Latent Image` batch size 를 16(frame 수만큼)으로 설정


## link
- [[controlnet]]
- [[comfyui]]
- [[fizznodes]]
- [[stable-diffusion]]
