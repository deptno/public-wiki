# stable-diffusion

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

## [[@todo]]
- [ ] 모델 확보
- [ ] voxel art, pixel art

## 구조
```sh 
models/stable-diffusion/* # model
```

## 용어정리 
- VAE | Variation Auto-Encoder
- CFG | Classifier-free Guidance
- Denoising strength | 0에 가까울수록 현재 이미지를 유지한다
- ControlNet | input 이미지를 통해서 생성이미지의 동작을 주입하는 플러그인
- LoRA | Low-rank Adaption for Fast Text-to-Image Diffusion Fine-tuning

## prompt
> 학습데이터에 매칭되는 키워드가 가지는 벡터와 관계가 있으므로 학습시에 사용된 AAA 게임 이름, 사이트 이름 그대로 영향을 발휘할 수 있다  
> 프롬프트 하나만을 입력했을 떄 결과가 원하지 않는 것이 나온다면 학습되지 않은 키워드일 수 있으며 이런 경우는 키워드를 변경해야한다

### 사용법
- **[prompt1:prompt2:ratio]** 형태로 sampling steps 을 기준으로 ratio 를 나눠서 사용이 가능하다
  ```
  beautiful [cat:bird:0.4]
  ```
  - 샘플링 20 스텝을 기준으로 8스텝까지는 고양이를 이후스탭은 새를 기준으로 생성한다
  - `[p1::0.5]`, `[:p2:0.5]` 형식으로 특정 스텝에서는 프롬프트를 제거하는 방식도 유효하다
- **(word:ratio)** 형태의 강조도 가능하다 `(cat:1.1)` 형태며 `1` 이하로 가게되면 효과가 비율만큼 약화된다
- 프롬프트는 앞에 나오는 순서대로 우선순위를 받는다
- 추가 script 를 통해서 outpaint(확대)  등에 대한 추가작업이 가능하다

### 대상
- 일반 명사, 형용사와 함께 작성

### 구도
- portratit

### 화질
> 동시에 사이트명으로 구글에 처보면 알 수 있다.  일종의 화풍과 비슷

- artstation
- deviantart
- trending artstation
- trending on deviantart
- pixiv ranking 1st

### 품질
- masterpiece
- best quailty
- concept artstation
- extreamly detailed
- ultra detailed
- brillant photo
- beautiful composition
- sharp focus
- 4k
- 8k
- ray tracing
- cinematic lighting
- cinematic postprocessing
- realism

### 기타
- kawaii cute girl
- cute eye
- small nose
- small mouth
- beautiful face
- brillant face
- perfect symmetrical face
- find detailed face
- aesthetic eyes

### 화풍
> stable-diffusion@2.x 버전에서는 유효하지 않을 수 있다

```
style of [화풍,화가] 
```

#### 스타일
- gothic
- renaissance
- baroque
- rococo
- chinoiserie
- romanticism
- realism
- victorian painting
- japonisme
- impressionism
- art nouveau
- cubism
- art deco
- surrealism
- pop art
- japanese anime

#### 애니메이션 
- cygames
- shinkai makoto
- kyoto animation
- a-1 pictures
- p.a. works
- atelier-ryza
- granblue fantasy
- genshin impact
- azur lane
- love live!
- final fantasyarknights

#### 페인팅 기법
- digital painting
- oil paintingwatercolor
- watercolor painting
- ink watercolor
- acrylic painting
- crayon painting
- pen art
- ball-point pen art
- drawing
- pencil sketch
- pencil drawing
- ukiyo-e painting
- etching
- pointillism
- pixel art
- stained glass
- woodcut
- bold line painting

### 시점과 빛
> 사진을 찍을때 피사체와의 거리 카메라의 옵션을 지정하는 것과 유사하다

#### 거리
- far long short
- long shot
- very wide shot
- wide shot
- medium shot
- west shot
- bust shot
- close up shot
- close up front shot
- close-up shot
- closeup
- head shot
- face closeup photo

#### 각도
- below view
- overhead view
- near view
- bird view
- selfie shot angle
- wide shot angle
- shot from a birds eye camera angle

#### 빛 및 구도 기타
- portrait
- snap shot
- landscape
- upper sunlight
- golden sun
- upper sunlight and golden sun
- beatiful composition
- sharp focus
- bright color contrast

#### 조명
- soft lighting
- cinematic lighting
- golden hour lighting
- strong rim light
- volumetric top lighting
- atmospheric lighting
- cinematic postprocessing top light
- brillant photo
- best shot
- beautiful background

### 세부 조작
> 얇은것들, 화살, 손가락 등은 실패할 가능성이 높으니 구도를 조절해야한다

- 큰 부부분 부터 세부적인 부분 순으로 작성
- 가중치 조정

## ControlNet
- 동작을 입히기 위해서 사용하는 것으로 동작이미지를 주입해서 생성이미지의 동작을 제한한다

## LoRA
- Low-rank Adaption for Fast Text-to-Image Diffusion Fine-tuning
- 추가학습을 통해 이미지 생성시 특정 성격을 주입할 수 있다
- 추가학습을 위해 사용될 키워드와 이미지를 통해 학습한다 학습 결과 파일은 `*.safetensors`
- 추가학습시 기존 키워드를 오염시킬 수 있으므로 이 경우에는 정규화 이미지를 준비해서 학습한다
  - 정규화 준비시 학습용:정규화 이미지의 비율은 **1:5** 정도가 적당하다는 의견이 있다
- 학습된 LoRA를 적용하면 특정 캐릭터, 고정된 스타일을 주입가능할 것으로 추측한다

## 사용
- txt2img
  - {positive,negative} prompt
  - sampling method: steps 이 높을수록 노이즈 제거 단계가 많다
    - `a` 로 끝나는 경우 무작위성이 높다, 정교한 이미지에는 적합하지 **않다**
    - `a` 로 끝나는 경우 무작위성이 높다 
  - Hires. fix
    - 생성후 img2img 로 고해상도로 변환
  - Batch count: 한번에 생성할 이미지 수 4정도로 시작해서 프롬프트가 유효한거같은데 16-64정도로 생산시작
  - Batch size: 동시에 생성하는 이미지 수로 병렬이지만 vram 소모가 크다
  - CFG Scale: 
- settings
  - face restoration: 얼굴 품질을 높임, 사진에 적합
  - tiling: 배경 무늬등에 반복할 이미지 생성

## 사용루틴
- 작은이미지로 빠르게 대량 생산
- 눈으로 확인
- 확대 ai
  + https://replicate.com/nightmareai/real-esrgan

## 이해
- 학습된 정보를 바탕으로 각각의 텍스트는 방향과 크기를 가지는 벡터로 저장된다
- 예를 들어 개, 고양이라는 단어는 각기 다른 고양이 같다는 방향과 크기로 저장
- 때문에 학습한 데이터에 따라 각기 단어에 매핑되는 벡터가 다르다 ->  결과가 다르다
- 노이즈를 먼저 생성하고 여기서 노이즈를 제거하면서 깨끗한 이미지를 만들어낸다
- 랜덤 시드는 이 노이즈 이미지와 관련되며 초기값이 다르면 다른이미지가 생성되게된다

## 모델
- 학습 모델에 따라서 화풍,  퀄러티가 변경된다
- 특정 목표를 가지고 설계된 모델들이 여럿 존재
- stable diffusion 모델은 1.x 에서 특정 화풍을 모방, 화풍 재현에 적합, `512x512` 로 학습
- stable diffusion 모델은 2.x 에서 모방 논란을 대응하여 더 창의적이고 디테일한 묘사가 가능 `768x768` 로 학습

## link
- [[ai]]
- [[midjourney]]
- [[comfyui]]
- [[python]]
