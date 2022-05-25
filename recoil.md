# recoil

글 작성 시점: 2022-05-24 v0.7 

- readonly -> value
- writable -> state

- atom
  상태값
  -> useRecoilState
  -> useSetRecoilState - updater 방식 사 용
- selector
  상태값에 기반을 두고 추가 처리를 한 데이터를 관리, atom 이 업데이트되면 다시 실행된다
  `get` 은 `Promise<T>`, `Promise<T>[]` 를 리턴해도 체인에서 유효
  -> useRecoilValue - set 이 내부적으로 쓰인경우 동작안할 것으로 문서화되어있음
  -> useRecoilState
- selectorFamily **curried**
  매개변수를 사용해야 할 때 사용
- waitForAll
  `Promise<T>[]` 처리 가능
- useRecoilCallback - **curried**
  `{snapshot, refresh, set}`, `snapshot.getLoadable(atom or selector)`
- useRecoilValueLoadable
  ReactSuspense 를 사용할 수 없다면 해당 함수를 이용하여 상태에 따른 컴포넌 처리가 가능
  - hasValue
  - loading
  - hasError
- useRecoilRefresher_UNSTABLE
  순수함수가 아닌경우
- atomFamily

```tsx
import { RecoilRoot } = 'recoil'

const FC = () => {
  return (
    <RecoilRoot>
    </RecoilRoot>
  )
}
```

## related
- [[react]]
