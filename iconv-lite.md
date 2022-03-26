# iconv-lite

buffer 로 처리해야한다.
```ts
import iconv from 'iconv-lite'

fetch.buffer().then((buffer) => {
  const buf = iconv.decode(buffer, 'euc-kr')
})
```

## related
- [[node]]
- [[encoding]]
