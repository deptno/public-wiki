# jwt
JSON Web Token

## 질문
### jwt 를 사용할때 refresh token 이 필요한지?
- 아래와 같은 형태의 사용도 가능하지 않은가?
1. jwt 토큰은 expire 이후에도 decode 자체는 가능하다
2. 만료된 token 을 decode 해서 expire 된 시간을 확인한다
3. 발급**했어야 할** refresh token 의 expire 기간내라면 access token 을 갱신한다

## link
- [[oauth]]
- [[jwe]]
