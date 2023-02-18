# typescript

## enable sourcemap
[[node]] 실행시에 sourcemap 을 활성화 해서 타입스크립트 코드로 확인이 가능하도록한다
```sh
tsc --sourceMap=true
node --enable-source-maps [source.js]
```

## build clean
[[vitest]] 테스트 중에 ts 와 빌드된 js 가 함께 있는 경우 js 를 먼저 읽는경우가 있어서 build 된 파일 제거 후 테스트가 필요했다.
```sh
tsc --build --clean
```
  + https://raghsonline.com/posts/typescript/typescript-clean-build/

## error
### compile option
```sh
/app/.pnp.cjs:10322
  return Object.defineProperties(new Error(message), {
                                 ^

Error: Qualified path resolution failed: we looked for the following paths, but none could be accessed.

Source path: /app/packages/lib/src/delay
Not found: /app/packages/lib/src/delay

    at makeError (/app/.pnp.cjs:10322:34)
    at resolveUnqualified (/app/.pnp.cjs:11847:13)
    at resolveRequest (/app/.pnp.cjs:11888:14)
    at Object.resolveRequest (/app/.pnp.cjs:11944:26)
    at resolve$1 (file:///app/.pnp.loader.mjs:1991:25)
    at nextResolve (node:internal/modules/esm/loader:163:28)
    at ESMLoader.resolve (node:internal/modules/esm/loader:838:30)
    at ESMLoader.getModuleJob (node:internal/modules/esm/loader:424:18)
    at ModuleWrap.<anonymous> (node:internal/modules/esm/module_job:77:40)
    at link (node:internal/modules/esm/module_job:76:36)
```
delay.js 가 아닌 delay 를 못찾겠다고 나오는 에러
컴파일 결과가 그러한 것인데 두가지 생각이 났는데 일단 되는 방향으로 처리
- [ ] module 로 컴파일 되엇으니 `.mjs` 를 찾는 것이 아닌지 해서 파일 명을 변경해본다.
- [X] `import` 구문 자체에 `.js` 확장자를 붙여서 임포트한다 
- [ ] TODO: 생각해보니 `module` 컨피그가 켜지면 directory import 를하고 그 안에서 `index.js` 임포트하는 구조인가 싶기도
일단 후자로 해결

- module: es2022
```typescript
import { a } from './module'
```
- expected
```javascript
import { a } from './module.js'
```
- actual
```javascript
import { a } from './module'
```
- solution
```typescript
import { a } from './module.js'
```
