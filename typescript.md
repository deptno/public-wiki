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

## void vs undefined
`void` 는 공허한 값으로 취급된다. 즉 값이 있다. 무슨 값인지는 모르겠으나 의미 없는 무언가를 리턴하며 이는 `unknown` 과 비슷한 성질을 가진다.
`undefined` 는 `undefined` 로 취급된다

```typescript
const f1 = () => undefined {
  return console.log()
}
```
- `undefined` 는 `undefined` 타입을 가진다
- console.log 의 반환 **값**은 `undefined` 이지만 **타입**은 `void` 을 가지기 때문에 타입에러가 발생한다
- `unknown` 타입과 비슷하지만 다만 명시적으로 **의미가 없는 값의 타입** 정도로 이해한다

## 유틸 타입 type
- NonNullable: null 제거
- Pick: 오브젝트에서 선택 속성으로 타입 생성
- Omit: 오브젝트에서 선택 속성 제거로 타입 생성
- Extract: 타입에서 추출
- Exclude: 타입에서 제거
- Required: 모든 속성에서 옵셔널 제거
- Partial: 모든 속성 옵셔널
- Parameters: 함수의 인자를 타입으로
- Awaited: Promise<T> | T

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

## version 변경사항
- 5.0
  - decorator
- 4.9
  - satisfied
  - auto-accessor: `#__[NAME]` 에 대한 `get`, `set` 확장 문법 슈거
  - `A|B` 타입으로 정의된 변수 `a` 대 해서 `[b에만 존재하는 속성]  in a`  는 `B`로 타입캐스트
- 4.8
  - `unknown` 타입은 `{} | null | undefined` 와 가가운 정의를 가진다
