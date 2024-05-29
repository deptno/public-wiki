# controlnet

## module
- temporalnet: 두 이미지간의 차이 보

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

## link
- [[stable-diffusion]]
- [[comfyui]]
