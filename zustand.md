# zustand

## 사용법
> 사용시에 아토믹하게 리턴하는게 중요, 오브젝트 리턴시에 참조 변수랑 무관하게 리렌더 발생
```js
const nuts = useBearStore((state) => state.nuts)
const honey = useBearStore((state) => state.honey)
```
  + https://github.com/pmndrs/zustand?tab=readme-ov-file#selecting-multiple-state-slices

## link
- [[react]]
