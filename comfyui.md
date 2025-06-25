# comfyui
- [[stable-diffusion]] ui
- 발전이 너무 빨라서 아래 내용은 기록으로만 참고

## model + node pair
| release date | model            | 설명 | url                                                 | node                                                     |
|--------------|------------------|------|-----------------------------------------------------|----------------------------------------------------------|
| 2025/04/02   | InstantCharacter | cref | https://github.com/Tencent-Hunyuan/InstantCharacter | https://github.com/jax-explorer/ComfyUI-InstantCharacter |


- [[@todo]]
  - [ ] composition 유지 이미지 생성
  - [ ] 배경합성
    - [ ] 배경 교체
  - [ ] 캐릭터 합성
    - [ ] 로라등으로 그리는방법
  - [ ] 인물 교체
    > 일론과 트럼프로 시작
    - [ ] 얼굴만
      - [ ] pulid
      - [ ] ipadapter
    - [ ] 몸까지
    - [ ] 옷까지
  - [ ] 학습
    - [ ] 1 학습
    - [ ] 2 학습
- [ ] 학습
  - [ ] 속도
    - [ ] wavespeed
    - [ ] sageattention
    - [ ] teacache
  - [ ] context 수정
    - [ ] icedit
    - [ ] 
  - [ ] 조명
    - [ ] iclight
  - [ ] 영상
    - [ ] wan
      - [ ] wan vace
      - [ ] causvid
      - [ ] moviigen
      - [ ] wan fusion x
    - [ ] ltx

## 튜토리얼
- [[youtube-playlist-pixaroma-comfyui-tutorial]]

> memory leak 이 있는 것 같다 한번 느려지기 시작하면 계속 느림

## module
- animatediff
  - animatediff models, lora 관련해서는 커스텀 디렉토리를 쓸 수 없다 symbolic link 를 쓰자;

## 확인사항
- [ ] `comfyui-cli` 라는 놈도 있음

## 설치
### ComfyUI
```sh 
# 파이토치 for apple sillicon
# + https://developer.apple.com/metal/pytorch/
$ pip3 install --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/cpu

# 가상환경 구성, 혹시나 해서 한번 해봄
$ pipenv shell

# 디펜턴시 설치
$ pip install -r requirements.txt
## or
$ pipenv run pip install -r requirements.txt

# 실행
$ python main.py # remote 접속허용을 원하는 경우 `--listen` 추가

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
- ui에서 queue 를 클릭하면 모델이 없다고 에러난다 아래 주소에서 다운로드후 `models/checkpoints` 디렉토리에 넣어준다
  + https://huggingface.co/runwayml/stable-diffusion-v1-5
- **Load checkpoint** 노드에서 넣어준 `checkpoint` 를 선택하고 `queue` 를 누르면 실행된다
  - m1max 64ram 기준으로 아무것도 건드리지 않고 기본 프롬프트로 **25.85s** 가 소요

### ComfyUI Manager
- 커스텀노드(플러그인) 관리
- ComfyUi 폴더에서 `custom_nodes` 디렉토리 진입후 클론한다
- ComfyUi 재시작 필요
```sh 
git clone https://github.com/ltdrdata/ComfyUI-Manager.git
```

### Lora

### [[controlnet]]
- 생성되는 이미지의 윤곽선을 제안할 수 있음, 자세 혹은 건물 생김세

### Stable Diffusion Video
+ https://huggingface.co/stabilityai/stable-video-diffusion-img2vid-xt/tree/main
- `ComfyUI-VideoHelperSuite` 설치 필요
- image to video
- *AnimationDiff* 의 경우 text to video

#### 커스텀 노드
| 세트 |                                              |                               |                                                      |                                                      |
|------|----------------------------------------------|-------------------------------|------------------------------------------------------|------------------------------------------------------|
|      | Canvas Tab                                   | 마스크 이미지 생성            |                                                      |                                                      |
| 0    | ComfyUI's ControlNet Auxiliary Preprocessors | 자세등 처리                   |                                                      | 리눅스에서는 추가적으로 파이썬 디펜던시 추가해줘야함 |
| 0    | ComfyUI_IPAdapter_plus                       | Image Prompt                  | https://www.internetmap.kr/entry/IP-Adapter-too-many | InsightFace 추가 필요                                |
|      | ComfyUI-SDXL-EmptyLatentImage                | 이미지 사이즈 설정시 유리     |                                                      |                                                      |
|      | ReActor Node for ComfyUI                     | 다른 얼굴로 바꾸기            |                                                      |                                                      |
|      | Face Detailer / (a1111: ADetailer)           | 얼굴 고치기                   |                                                      |                                                      |
|      | InstantID                                    | IPAdapter 와 유사 + 인물 유지 |                                                      | InsightFace 추가 필요, SDXL 전용                     |
|      | UltimateSDUpscale                            | 설정을 가진 업스케일러        |                                                      |                                                      |
|      | pythongosssss/ComfyUI-Custom-Scripts         | 편의기능 + workflow export    |                                                      |                                                      |
|      | ComfyUI-VideoHelperSuite                     |                               |                                                      | SVD, animationDiff 에서 필요                         |
|      | ComfyUI-Crystools                            | 컴퓨터 자원 모니터링 기능     |                                                      |                                                      |


##### [[ControlNet]]
- IP Adapter 등을 사용할때 text prompt 가 제대로 동작하지 않으면 `weight` 를 0.7정도로 낮춰서 사용해본다
- IP Adapter
  - `Image Prompt` 로 현재 이미지에 대한 Text Prompt 가 입력된것으로 간주된다라고 생각하면 이해가 편하다
  - `Plus` 가 들어간 모델은 더 많은 토큰을 써서 원본을 최대한 유지
- Open Pose  
  - 이미지에서 자세를 분석해서 자세를 적용한다
- `Plus Face` 옵션은 얼굴 유지
  - 인페인트 전용 체크포인트를 사용한다면 효과가 좋다
  - 인페인트 칠한 부분만 IP 가 적용된다
- 모델 다운로드 후
  - /ipdapter 생성 모델 복사
  - /clip_vision 생성 후 encoder 모델(CLIP vision 에서 사용됨) 복사(이름 변경 필요)
- custom_nodes
  - Apply IPAdapter
  - Load IPAdapter Model
  - Load CLIP Vision
  - Batch Images - 이미지를 여러개 섞어서 입력하려는경우
- `Apply ControlNet` 은 text prompt 의 positive 를 입력으로 받아 `KSampler` 로 아웃된다
- `*Preprocessor`, ControlNet 을 적용하기 위해 이미지의 특성을 추출하여 이미지를 생성한다

##### Upscale
- 업스케일
- 얼굴 복구를 위해서도 씀

##### ReActor 
- 얼굴을 다른 사진의 인물과 바꿔준다
- 여러명 가능
- 비디오도 가능
  - `ComfyUI-N-Nodes` 설치 필요

##### InstantID 
- SDXL (Turbo) 전용
- 사진 -> embedding -> keypoint(얼굴정보 추출)
- 눈 코입의 위치만 지정하므로, 원본 사진과 비슷한 표정을 하려면 text prompt 로 묘사를 추가한다
- 설정
  - `Control Weight` 0.5
  - `Ending Control Step` 0.4 ~ 0.6
  - 사용하는 checkpoint 마다 추천 설정이있으니 확인할 것

### SDXL Lightning
- checkpoint 1,2,4,8 step 용
- LoRA 도 제공, 이건 SDXL Lightning 대신 SDXL 모델을 사용하면서 LoRA 로 동일한 효과를 낼 수 있도록 함

## 사용
- ComfyUI Manager
  - 컴스텀 노드 사용을 위해 사용됨
    - 커스텀 노드는 일종의 익스텐션
- AI 로 생성된 이미지를 드래그해서 넣으면 생성 노드가 나타남
- 그룹화 가능
- 노드이름 변경가능
- txt2img -> `KSampler` 에 입력되는 `CLIP` 에 의해 결정
- img2img -> `KSampler` 에 입력되는 `latent_iamge` 에 의해 결정
- `input` 디렉토리에 이미지 넣어두면 기본로드가 가능

### 체크 포인트 사용
- 모델마다 설정값이 다르니 참고

### 여러프롬프트를 사용해서 이미지 합성
- `Conditioning (Set Area)` -> `Conditioining (Combine)` 노드로 합성 후 샘플링
- 주로 배경

### denoising 중인 이미지 여러개의 이미지를 중간에 하나로 합쳐서 생성
- `KSampler Advanced` 를 사용하여 denoising 중인 이미지를 `Latent Composite` 노드를 통해 합성
- 주로 배경 + 인물

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

### 옵션
- CFG
  - 프롬프트를 얼마나 따를것인지, **높을수록 따름**
- sampler
  - lcm 로라 사용지 lcm 샘플러 사용
    - a1111 의 경우 lcm 샘플러는 `AnimationDiff` 에 포함되어있음
- KSampler:`denoise` -> 값이 높을 수록 랜덤성을 부여

## [[error]]
### `DWPose: Onnxruntime not found or doesn't come with acceleration providers, switch to OpenCV with CPU device. DWPose might run very slowly`
```sh 
UserWarning: DWPose: Onnxruntime not found or doesn't come with acceleration providers, switch to OpenCV with CPU device. DWPose might run very slowly
```
- onnxruntime 이 없어서 가속을 받지 못하는 문제 관련 패키지 설치
```sh 
pip install onnxruntime onnxruntime-gpu
```
- 확인
```sh 
DWPose: Onnxruntime with acceleration providers detected
```

## link
- [[stable-diffusion]]
- [[stable-diffusion-recipe]]
- [[stable-diffusion-webui-forge]]
