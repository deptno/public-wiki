# next-atuh

- 인증 처리를 위함 [[auth.js]] 로 마이그레이션을 발표한지 시간이 좀 지났다 2023-12-09 현재 1년 이상 정도

## model
+ db model 참고 https://authjs.dev/reference/core/adapters
+ user 1:n account(oauth provider)
+ user 1:n session(oauth provider)
+ user 1:n verification_token(oauth provider)

## try next-auth@beta
+ [[diary:2023-12-09]]
- 문서가 auth.js, next.js + pages, next.js + app router, next-auth, next-auth@beta(5), auth.js 가 섞여있어서 파악이 어렵다
- 현 시점 기준으로 `next-auth@beta` 패키지를 사용해본다
- app router 에 컨벤션에 맞추기 위해서는 auth.js upgrade guide (v5) 를 참조
  + https://authjs.dev/guides/upgrade-to-v5
- adapter는 `@auth/*-adapter` 로 이관이 완료되어 이 패키지를 쓰면된다

### auth()
- server 에서 사용되며 `session` callback 은 무시되고 `jwt` callback 결과  혹은 `database` strategy 인 경우 `User` 모델이 리턴되는 것으로 이해
- server: `auth()`, client: `useSession()` 으로 이해

## link
- [[nextjs]]
