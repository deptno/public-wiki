# turborepo
+ https://engineering.linecorp.com/ko/blog/monorepo-with-turborepo

## cache invalidation
- `--force` 캐시를 안읽고(무시) 결과는 캐시함
- `--no-cache` 캐시를 안함

## turbo.json

### root level
- `globalEnv` 환경변수 변경시 캐시 무효화

### task level
- `env`
  - 해당 환경변수가 변경된 경우도 캐시 무효화
- `inputs` 기본 값은 [] 로 아무파일이나 변경해도 캐시 무효화
  - 변경시 해당 파일이 변경된 경우만 캐시 무효화, 기본값(전체)를 유지하면서 제외하는 방식으로 갈때는 `$TURBO_DEFAULT$` 라는 키를 통해 기본값 명시 가능
- `persistent` 는 실행이 종료되지 않는 커맨드가 있는 경우 표현해주면 다른 스크립트에서 해당 디펜던시 무시
- `cache` 는 default 가 `true`
- `outputs` 는 캐시된 아티팩트에 포함

## link
- [[pnpm]]
- [[nextjs]]
- [[monorepo]]
