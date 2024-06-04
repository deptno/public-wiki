# controlnet

## 개념
- text -> token -> embedding 이 진행되는데 이 중간에 적용되어 최종 embedding 에 영향을 미친다
- 이미지는 각각의 preprocessor 를 통해 필요한 이미지를 output 으로 낸다
- text 토큰을 입력과 preprocessor 를 거친 이미지를 controlnet 모듈이 입력받아 embedding 을 형성한다

## 설치
### [[comfyui]]
- *ComfyUI-Advanced-ControlNet* 설치
  - 모델은 추가로 설치 필요
    + https://comfyanonymous.github.io/ComfyUI_examples/controlnet/
      - 모델 자체 repo 는 위에서 찾아보는것이 최신링크 파악에 도움이 될 것
    - 현재 기준 최신(v1.1) 모델 파일
      + https://huggingface.co/lllyasviel/ControlNet-v1-1/tree/main
      - 허깅 페이스 주소를 `/models/controlnet/` 아래 [[git-clone]] 가능
        ```sh 
        cd models/controlnet
        git clone https://huggingface.co/lllyasviel/ControlNet-v1-1 
        ```
- *ComfyUI's ControlNet Auxiliary Preprocessors* 설치
  - preprocessor 를 사용하기 위해서 설치 필요
  + https://github.com/Fannovel16/comfyui_controlnet_aux
  - linux 에서는 [[comfyui]] manager 를 통한 설치 외 `requiements.txt` 의 dependency 설치가 추가로 필요

## module
- temporalnet: 두 이미지간의 차이 보간

## 선행처리기
- 이미지 -> 선행처리기 -> 이미지 컴포지션 -> 모델 -> 적용

## 모델
- canny - 엣지 디텍팅
- lineart - 연필로 밑그림 그린듯한
- depth - 가까우면 흰색 멀면 옅은색
- normal - 3가지 색상으로 3차원 표현
- mlsd - 직선감지
- reference - controlnet 을 사용하지 않고 이미지 스타일 참조
- scribble - 낙서, 손으로 표현할수있또록
- seg - 지정된 분류에 따라 각각의 군집을 특정 색상으로 표현

## link
- [[stable-diffusion]]
- [[comfyui]]
- [[animatediff]]
