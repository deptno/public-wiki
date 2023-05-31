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

## related
- [[react]]
- [[tailwindcss]]
- [[yarn]]
- [[corepack]]
