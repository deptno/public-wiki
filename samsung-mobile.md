# samsung-mobile|삼성모바일

## 트러블 슈팅
### 다크모드에 흰색 배경 적용
삼성 모바일 버전: 21
- 해결방안 테스트
  - [ ] `background-color: white`
    - 하얀색 계열 자체가 동작하지 않는다.
  - [ ] meta.color-scheme 적용
    + https://jizard.tistory.com/421
      - zflip 3, android 13 기준으로 동작하지 않는다.
```css
background-color: white;
```

`red` 나 `green` 은 동작하는데 `white` 계열은 동작하지 않는다 유저가 [[darkmode|다크모드]]를 선택한 만큼 눈의 피로를 줄이기 위해서 그랬을 수 있다는 생각이 든다.
  - [ ] [[css]] 의 prefers-color-scheme 를 통해 제어한다
    + https://developer.mozilla.org/ko/docs/Web/CSS/@media/prefers-color-scheme
    - 이 부분이 정식 솔루션이 될텐데 동작하지 않았다.
  - [ ] background-image 1x1 반복적용
  - [ ] 이미지 하얀 배경 부분 크기 확장
  
- [[@todo]] `prefers-color-scheme` 가 왜 동작하지 않았는지 찾아볼 [[필요가]] 있다. 
- darkmode 처리를 위해서는 백그라운드 색상을 적용할 이미지를 적용할때 이러한 상황이 먼저 고려되고 테스트되어야한다.

## link
- [[browser]]
- [[css]]
