# nextjs

next.js

## next 13 app directory setup
```sh
yarn dlx create-next-app@latest --experimental-app [project]
cd [project]

corepack enable
yarn set version stable

yarn config set nodeLinker pnpm # 필수는 아님, 기본은 node_modules
yarn # migrate to yarn 3.x
```

### tailwindcss
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

#### ui framework based on tailwindcss (eg. daisyui)
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
```txt
Error: This action with HTTP GET is not supported by NextAuth.js
```
`--turbo` 옵션을 빼니 동작한다. dynamic route 쪽에 문제가 있는 것으로 보인다. 버전 정보를 추가해 둔다.
```json
    "next": "13.1.6",
    "next-auth": "^4.19.2",
```



## related
- [[react]]
- [[tailwindcss]]
- [[yarn]]
- [[corepack]]
