# dataloader

## 코어 피쳐
- 배치
  - 각각의 요청을 모아서 nexttick 타이밍에, 배치 요청을 함수를 호출하여 한번에 데이터를 가져온다
- 캐싱
  - 요청에 대한 값을 캐싱하여 디비에 두번 쿼리하지 않는다

## 사용을 위해 라이브러리
+ **깃허브** https://github.com/deptno/dataloader-toolbox

### 설명
> 사용했던 dataloader-toolbox 코드를 기준으로 의미를 역추적하여 그럴것이다라는 추론으로 빠르게 정리

- 요청당 dataloader session 을 생성
- session 은 `WeakMap` 형태, function 주소를 key 값으로 가짐
- database query 요청 함수에 대해서 dataloader **wrapper** 를 생성하고 session 에 저장
  - original 함수는 배치 처리를 위한 `T[]` 인자를 받는 함수
  - **wrapper** 는 `.load` 메소드를 통해서 `T` 를 인자로 받는 함수 
- 해당 함수를 resolver 의 여러 곳에서 호출하게될 수 있는데 이때 마다 wrapper 를 획득하여 호출(캐싱처리)
- 해당 요청이 모여 nexttick 에 배치 처리를 하게되면서 실제 함수인 original 함수가 호출
- 응답이 각 **wrapper** 의 호출부였던 `.load(T)` 에 응답 분배됨

## link
- [[graphql]]
