# stable-diffusion
> diffusion 기술을 사용한 이미지 생성 ai

## [[frontend]]
- [[comfyui]]
- [[stable-diffusion-webui]]
- linux 에서 설치시 nvidia toolkit 설치가 필요하다 
  - [[wsl]]
    - wsl 도 리눅스라 설치 안했더니 gpu 사용율이 나오지 않음
    - 재부팅후 중간에 설치하면서 이미지 생성해보니 속도가 빨라져있음, 설치 중이라 재부팅 이펙트와 어떤 것이 맞는지 확인 불가
    - 해당쉘에서 이미 `nvidia-smi` 커맨드 사용 가능
    - `nvidia-smi` 커맨드가 있는 것을 먼저 확인 후 없는 경우 [[cuda]] toolkit 설치를 진행하면될 것

## 주요 모듈
- [[controlnet]]
  - 모듈 하부로 여러 모델이 존재한다 대표적인 것 리스트
    - openpose 캐릭터의 뼈대 및 표정을 캡쳐하여 이미지로 출력한다
    - softedge 이미지에서의 선들을 표현한다
  - 이런 하부 모듈들을 합친 결과를 임베딩으로 표현하여 다음과 같은 결과를 얻는 사용이 가능하다
    - video to video 영상 카피
    - 같은 포즈 캐릭터 이미지 생성
- [[animatediff]]
  - text to image 을 통해 애니메이션 생성
  - 텍스트 프롬프트를 통해서 애니메이션을 생성
  - 하위 모델과 로라를 통해 애니메이션시에 카메라 앵글등 조절 가능
  - 모션 모듈
    - t2i 시에 어떻게 이미지화 될지를 결정하는 것으로 추측
    - temporaldiff 같이 시간 변화에 따른 frame 간의 일관성에 가중치를 둔 커스텀 모듈도 존재
  - 모션 로라 모듈
    - 모션 산출시 카메라 앵글등 조정
- stable-diffusion-video|svd
  - image to video, image 를 넣고 돌리면 짧은 애니메이션이 생성됨
- detailer
  - 무너진 디테일을 보정하기 위해 사용된다
  - 손이나 얼굴과 같이 특정 부분의 디테일을 보정한 이미지를 산출
  - 부족한 디테일을 추가하기도
- refiner
  - 색상, 명암, 노이즈 감소에 집중하여 퀄리티를 개선한다
- upscailer
  - 이미지를 확대하면서 동시에 퀄리티도 상승시킨다
- ipdapter
  - image prompt 라고 이해하면 될 듯
  - `image -> 특성 추출 -> token` 화 하는 방식에서 자체적으로 인코딩을 하는 방식으로 변경되고 있는 듯
  - ipdapter face id 등의 확장이 존재하며 이미지로 부터 타겟 이미지의 얼굴만 인페인팅하여 카피하는 방식의 사용케이스가 주가 될 것 같음
- [[rvm]]
  - 주요 모델이라기 보다도 video to video 복사를 할때 캐릭터 마스킹을 위해 사용될 것으로 예상
  - 비슷한 기술로 rmbg 라는 이미지 백그라운드 제가 기술이 존재

## video 를 만드는 과정
- image to video
  - stable-diffusion-video
- text to video
  - [[animatediff]]
- video to video
  - video 는 image 의 연속이므로 이미지 마다 작업을 진행해서 이어 붙이는 개념
  - controlnet 을 활용하여 이미지마다 동작과 선을 따서 생성후 이어 붙인다
  - 이 과정에서 필요한 이미지간의 일관성을 위해 temporalnet, temporaldiff 등이 사용된다
  - 일반적으로 memory 이슈로 200 프레임 이하의 이미지를 배치로 생성하게
  - 배치 생성시에는 10 프레임정도를 추가로 뽑아 합칠때 겹쳐두면 퀄리티에 도움이 됨
  - 이를 영상 편집도구를 통해서 합치거나 하면될 것

## game asset 생성
- lora 와 프롬프트를 통해 특정 스타일 일관성을 생성
- text 삽입의 경우 밑바탕 이미지를 그린 후controlnet 을 통해 생성하면 도움이 됨

## 모델 구하기
- civitai
- [[huggingface]]

## 용어
- vae
  - 일종의 색감을 입히는
- cfe
  - prompt 를 얼마나 따를 것이냐
- denoise
  - diffusion 이 noise를 깔고 제거하는 방식임을 상기하면 노이즈 제거 강도는 새로운 이미지로의 변환으로 봐도된다

## link
- [[ai]]
- [[midjourney]]
- [[comfyui]]
- [[python]]
- [[book/making-game-graphic-with-stable-diffusion]]
- [[huggingface-ml-for-games]]
- [[animatediff]]
- [[stable-diffusion-recipe]]
