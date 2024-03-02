# next.js

next.js

## next
### next@13
- client side cache 를 가지 server 에서 받아온 컴포넌트들을 재활용한다
- partial rendering 
  - client navigation 이 일어난 경우 해당 컴포넌트 부분만을 새로 그린다
- directory 에 page.tsx 가 있어야 라우팅이 가능해진다
- route groups
  - `(name)` directory
  - url 구조에 영향을 주지 않고 `layout` 등 공통 컴포넌트 사용을 가능하게 한다
  - 다른 route groups 에 같은 라우트(url) 을 생성하는 경우 에러로 간주된다
  - route groups 간의 navigation 은 full page load 가 발생한다
- dynamic route
  - `/[name]/page.tsx` 의 경우 `({params}: {params: { name: string }})` 을 받게 된다
  - catch all segments
    - `/[...name]/page.tsx` name 이 하나의 뎁스가 아니라 여러 뎁스인 경우에도 매칭된다
    - `({params}: {params: { name: string[] }})`
  - optional catch all segments
    - `/[...name]/page.tsx` `name` 이 없는 `/` 의 경우에도 매칭된다
- page.tsx 는 기본적으로 server component 지만 `'use-client'` 를 통해 client component 사용이 [[가능]]
- layout.tsx 
  - 중첩가능
  - subtree 에서 재사용
  - subtree 라우팅시 re-render 되지 않는다
  - root layout 이 아니라면 optional
  - root layout (최상위 layout.tsx) 는 required
  - root layout (최상위 layout.tsx) 는 <html>, <body> 태그를 포함해야한다
  - root layout 의 경우는 server component 만가능하다 나머지는 client component 로 활용이 가능하다
  - fetch 가 가능하다
  - layout fetch 는 자식에게 전달될 수 없지만 children 에서 중복 패치를 하더라도 next.js 가 퍼포먼스 영향 없이 중복을 제거하여 처리
- template.tsx
  - client component 로 추측된다
  - 위치상으로는 layout 의 child
  - 마찬가지로 중첩이 가능하다
  - navigation 시 재사용되는 layout 과 달리 mount 시점마다 다시 instance 화 된다
  - 둘 중 하나를 쓰는 것이라면 layout 이 더 권장된다
- navigation
  - <Link> - server and client components, 추천 됨
  - useRouter - client components
  - 조건이 안되서 hard navigation 이 일어나는 경우 loading.tsx 가 호출됨
- server components 는 client side 에서 cached 된다
  - `useRouter().refresh()` 를 통해 invaliding 이 가능하다
- <Link> 컴포넌트가 viewport 안에 들어오면 기본적으로 prefetch 된다
  - prefetch 는 default 가 true
  - dynamic route 의 경우에는 loading.tsx 만 먼저 prefetch
- cache
  - if cache soft, navigation
    - static route
    - dynamic route
      - [path] 가 변하지 않은 경우
    - back/forward navigation
  - if cache is invalidated, hard navigation
- next.js 는 focus and scroll management 를 함

tbd
- parallel routes - 2 page 를 한번에 그린다(e.g. dashboard)


### next@13 app directory setup
```sh
yarn dlx create-next-app@latest --experimental-app [project]
cd [project]

corepack enable
yarn set version stable

yarn config set nodeLinker pnpm # 필수는 아님, 기본은 node_modules
yarn # migrate to yarn 3.x
```

### server and client components
+ https://beta.nextjs.org/docs/rendering/server-and-client-components
- server component 여러곳에서 같은 data fetch 는 캐시를 통해 공유되어 한번만 fetch 가 일어난다
- client component 에서 server component 를 렌더링할 수 없다
  - client component 에서 children 으로 server component 를 받아서 렌더링한다
- server component 에서는 client component 렌더링이 가능하다
- client component 는 최상단에 `'use client'` 디렉티브 선언이 필요하다
  - 기존 많은 라이브러리들은 디렉티브가 없으므로 **server component 에서 사용하는 경우** 래핑이 필요하다
### static and dynamic rendering
- 아래 기능을 사용함으로써 동적 렌더링을 사용할 수 있다
  - dynamic function
    - server component
      - cookies()
      - headers()
    - client component -> <Suspend /> 사용 권고 (loading.ts)
      - useSearchParams()
  - dynamic data fetching
    - dynamic
      - no-store
      - revalidate: 0
  - 정리
  | rendering | data fetching | dynamic functions |
  |-----------|---------------|-------------------|
  | static    | cached        | no                |
  | dynamic   | cached        | yes               |
  | dynamic   | not cached    | yes               |
  | dynamic   | not cached    | yes               |


```ts 
export const dynamic = 'auto'
// 'auto' | 'force-dynamic' | 'error' | 'force-static'
```
  + https://beta.nextjs.org/docs/api-reference/segment-config#dynamic

## tailwindcss
```sh
yarn add -D tailwindcss postcss autoprefixer
yarn tailwindcss init -p # -p 는 postcss 설정 추가

cat app/globals.css << EOF # hello
@tailwind base;
@tailwind components;
@tailwind utilities;
EOF

cat tailwind.config.js << EOF
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./app/**/*.{ts,tsx}",
    "./pages/**/*.{ts,tsx}",
    "./components/**/*.{ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
EOF
```

### ui framework based on tailwindcss (eg. daisyui)
```sh
yarn add daisyui
cat tailwind.config.js << EOF
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./app/**/*.{ts,tsx}",
    "./pages/**/*.{ts,tsx}",
    "./components/**/*.{ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [
    require('daisyui'),
  ],
}
EOF
```

## config
### standalone
- next.js@13.2 standalone 적용 후
  - image size: 465 -> 363 Mb 으로 빠짐 -> 22% down
  - memory usage: @ 는 website 에 한번 접속 후의 상태
    - 69@129Mi -> 24@65->83Mi 
    - 69@117Mi -> 25@64->108Mi
    - 최적화가 조금 더 들어가는건지 메모리 사용량이 빠짐

## cache
+ https://nextjs.org/docs/app/building-your-application/caching

### Request Memoization
> 클라이언트 당 한번의 요청에서 발생하는 캐시 컨트롤
- **React Component Tree** 내에서 요청당 같은 요청에 대해서 캐싱을 한다 - [[../dataloader|dataloader]] 와 같은 개념
  - [[../nextjs|nextjs]] 가 아닌 [[../react|react]] 피쳐다
  - fetch 의 `GET` 에서만 동작
  - route handler 에서는 동작하지 않는다 *GET* 혼동하지 말것, 여긴 리액트 컴포넌트 트리의 일부가 아니기 때문
  - 특수한 처리를 위해서라면 `React.cache` 함수를 통해 커스텀 처리 가능
  - opt-out 가능, 할일은 없을 듯
    - `fetch(url, { signal: new AbortController() })`

### Data Cache
> Request Memoization 과는 달리 영구적으로 캐시된다
- 렌더링 중에 만난 `fetch` 는 기본적으로 캐시된다
- `fetch(..., { cache: 'no-store' })` 로 캐시를 무시하고 요청할 수 있으며 이 또한 **캐시**된다
- 캐시는 영구적이며 두가지 캐이스로 revalidation 이 가능하다
  - 시간 지정(`{next: { revalidate: 60 }}`)
    - **중요한 점**은 시간을 60초로 지정했을때 90초가 지난 시점에 요청이 **처음** 들어왔따면 이때 revalidate 가 진행되고 리턴 자체는 캐시되었던 데이터가 리턴된다. `stale-while-revalidate` 와 개념이 같다
  - 명시적 요청
    - `revalidateTag('tag')` 
      - `{next: { tags: [tag] }}` 요청에 대해서 무효화 한다
    - `revalidatePath`
- `fetch` 메소드를 명시적으로 쓸 수 없어서 옵션을 전달할 수 없는 경우 *Soute Segment Config*을 사용한다
  + https://nextjs.org/docs/app/building-your-application/caching#segment-config-options
- opt-out 가능
  - `export const revalidate = 0`
  - `export const dynamic = 'force-dynamic'`

### Full Route Cache
> 빌드타임에 생성되는 리액트 서버 컴포넌트 페이로드와 클라이언트 컴포넌트에서 자바스크립트를 통해 생성하는 html
- 빌드 타임 캐싱, or revalidation
- static rendering 은 빌드 타임에 기본적으로 캐시된다
- dynamic rendering 은 빌드 타임에 기본적으로 캐시되지 **않는다**
- [X] ~~파라메터(slug) 가 포함된 페이지의 경우 `generateStaticParams` 에 정의경우 빌드 타임 아니면 런타임의 첫 요청에서 캐싱됨~~
  - 불확실
    + https://nextjs.org/docs/app/building-your-application/caching#generatestaticparams
    - [ ] 위 내용에 따른 dynamic 도 첫 요청떄 캐시를 하는데 `generateStaticParams` 이 명시된 곳에서 빠진 경우에만인지 확인이 필요
- chunk
  - chunking
    - route 마다
      - React Server Comoponent -> **React Server Component Payload** 로 변환
        - streaming 을 최적화하기 위한 *바이너리* 포맷
        - rsc 의 결과로 dom 을 업데이트하기 위해 사용
        - 클라이언트 컴포넌트 placeholder
        - 클라이언트 컴포넌트로 전달될 props
        - **클라이언트**의 **Route Cache** 에 저장됨
        - **Route Cache** 에 존재하는 경우 **요청 자체를 스킵**
    - suspense 바운더리 마다
  - 둘을 합쳐서 청크 생성
    - React Server Component Payload,
    - Client Componetn javascript to render html on the server
- 무효화
  - `fetch(url, { signal: new AbortController() })`
  - 새로운 빌드 배포
- opt-out
  - dynamic function 사용
    - cookies 
    - headers
    - searchParams, 이건 prop
  - cache 되지 않은 `fetch` 를 사용하는 경우 해당 `fetch` 만 opt-out 된다 나머지 자체는 캐시가 유지된다

### Route Cache
> 방문했던, 방문할(prefetching) 라우트에 대한 클라이언트 캐시  
> React Server Component Payload 를 route segment 마다 in-memory 저장  
> user session 동안 유지
- 지속시간
  - static 페이지인 경우 5분
  - dynamic 페이지인 경우 30초
    - `<Link prefetch />` 혹은 `router.prefetch` 를 명시적으로 사용하는 경우 5분
- 무효화
  - `router.refresh`
  - server action
    - `revalidateTag`
    - `revalidatePath`
    - `cookies.set`
    - `cookies.delete`
- opt-out
  - 얜 opt-out 이 없다

### Full Route Cache vs Route Cache

|        | Full Route Cache    | Route Cache            |
|--------|---------------------|------------------------|
| 어디서 | Server              | Client                 |
| 무엇을 | RSCP & HTML, static | RSCP, static & dynamic |
| 얼마나 | persist             | user session           |


## error
### next.js 13 + next-auth
signin 을 누르면 제대로 동작하지 않는이슈 브라우저에서는 아래 메시지가 찍힌다
```sh
next dev --turbo

[400] /api/auth/providers (194ms)
[400] /api/auth/_log (30ms)
[400] /api/auth/error (97ms)
```
아래는 브라우저에서 찍히는 로그
```text
Error: This action with HTTP GET is not supported by NextAuth.js
```
`--turbo` 옵션을 빼니 동작한다. dynamic route 쪽에 문제가 있는 것으로 보인다. 버전 정보를 추가해 둔다.
```json
    "next": "13.1.6",
    "next-auth": "^4.19.2",
```

### next-auth/src 를 참조해서 에러나는 경우
참조가 src로 걸린건지 확인해서 수정할 것
```sh
./node_modules/.store/next-auth-virtual-8ec2bd5fde/node_modules/next-auth/src/core/lib/assert.ts:134:27
Type error: Element implicitly has an 'any' type because expression of type 'string' can't be used to index type 'Adapter<boolean>'.
  No index signature with a parameter of type 'string' was found on type 'Adapter<boolean>'.

  132 |       "useVerificationToken",
  133 |       "getUserByEmail",
> 134 |     ].filter((method) => !adapter[method])
      |                           ^
  135 |
  136 |     if (missingMethods.length) {
  137 |       return new MissingAdapterMethods(
```

### 'sharp' is required to be installed in standalone mode for the image optimization to function correctly.
```sh 
⨯ Error: 'sharp' is required to be installed in standalone mode for the image optimization to function correctly. Read more at: https://nextjs.org/docs/messages/sharp-missing-in-production
```
- arm 에서 빌드하고 x86 에서 [[docker]] 컨테이너가 뜨게되는데 경우 네이티브 빌드 모듈인 `sharp` 없다고 `warning` 이 난다
- dockerfile 에 글로벌 설치해서 처리해버리는게 가장 깔끔하다 생각된다
```dockerfile 
RUN npm install -g --arch=x64 --platform=linux --libc=glibc sharp
ENV NEXT_SHARP_PATH=/usr/local/lib/node_modules/sharp
```
  + https://github.com/lovell/sharp/issues/3877#issuecomment-1850388322

## link
- [[react]]
- [[tailwindcss]]
- [[yarn]]
- [[corepack]]
- [[next-auth]]
