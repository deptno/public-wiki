# oauth
- google, facebook, naver, kakao 등 3자 서비스 인증을 통해 로그인

## 인증
### 신뢰, 비신뢰 클라이언트 인증
- 민감한정보에 클라이언트가 접근할 수 있으면 비신뢰 분류
- web: cookie, storage 등에 유저가 접근가능
- app: asyncStoarge 등은 유저가 접근 가능
  - 키체인등은 유저가 접근 못하므로 **신뢰기반으로 분류가 가능**

### offline access 
- offline access 의 의미는 유저의 액션 없이도 서버에서 접근한다는 의미로 이해
- 신뢰 클라이언트 인증에서 이루어짐

### access token
- api 를 통해 data access 하기 위해사용
- [[jwt]] 형태 사용 가능

### refresh token
- access token 만료에 따른 요청시 refresh token 으로 access token과 refresh token 을 갱신(재발급)
  - 유저 입장에서는 refresh token의 만료 시간 이내에만 지속적으로 접근한다면 추가 인증없이 지속적 사용 가능
- 신뢰 클라이언트 인증에서 사용
  - 비신뢰 클라이언트에서도 사용 가능하나 탈취시 refresh token 의 유효기간만큼 위험이 존재

## 고급 주제
### token 발급
- 형태는 [[jwt]] 를 기준으로 설명
- 암호화는 일반적으로 [[jwe]] 를 사용
- 발급할때 secret 으로 서명 하게되는데 access token, refresh token 을 *동일* secret 으로 서명
  - access token 은 expire 기간을 *짧게* + claim 정보
  - refresh token 은 expire 기간을 *길게*

### custom claim 추가
> *추정* oauth 인증을 통해 얻은 claim 외에 추가적인 정보를 token 에 주입하기 위한 과정  
> 처음에 생각을 잘못해서 길을 어렵게 갔는데 그럴필요없이 api 를 분리해서 편하게 가는게 복장성 측면에서도 나은것 같다  
> 2018즈음 첫 구현 당시에 [[serverless]] 기반의 구현을 하다보니 어려운 고민을 했던 기억

- 선택
  > 유저 정보를 예로 든다면 토큰에 클레임으로 주입할지 api 로 분리해서 추가 콜을 할지 선택할 수 있다
  - oauth 인증을 통해얻은 토큰을 [[jwt]] 로 한번더 래핑해서 유저정보등 클레임을 주입
    - [[next-auth]] 에서 동일 역할을 해준다
      + https://github.com/nextauthjs/next-auth/blob/0126f94788a263bd8420ceac9a11ed6d2c2fb958/packages/core/src/lib/actions/callback/index.ts#L121-L152
    - 인증된 oauth 토큰으로 유저 정보에 대한 api 를 생성해서 추가 정보에 접근할 수 있도록 함

## 질문
### access token 이 jwt 로 생성될때 refresh token 발급 필요성
- [[jwt#jwt 를 사용할때 refresh token 이 필요한지?]] 참고

## link
- [[jwt]]
- [[authentication]]
- [[next-auth]]
