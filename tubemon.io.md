# tubemon.io

## :TODO:
+ [[diary:2025-02-08]]
+ https://github.com/deptno/salji.ro/commit/52fea0fe15a86fa7b426a301f6422066f2472413
- [[corepack]], [[pnpm]] 관련 이슈, resolve 될때까지 유지

## 업무
- [[eos]] [[diary:2024-05-12]] [[aws]] [[eventbridge]] 12개 규칙 stop

## 장애
### [[meilisearch]] 죽음
+ [[diary:2024-05-22]]
- 1년만에 갑자기 사망 데이터가 깨진것인지 무한 [[pod]] 재시작
- `1.1.1` 버전이었는데 어떤 버전으로도 업그레이드를 해도 데이터 마이그레이션이 필요
- [[deployment]] 제거 후 새로운 버전으로 만들고 다시 데이터 붓는 방식으로 처리하기록함
- 다시 붓는 cronjob 은 이미 만들어뒀었음

### 정상적인 채널들이 `ban` 되고 있는 문제 :2025-02-08:
+ [[diary:2025-02-08]]
- 정상 적인 채널이 ban 되고 있는 문제 수정
  + https://github.com/deptno/salji.ro/pull/649
- 기존 정상적인 채널이 `ban` 된 것 찾아 `5005` 개 채널 복구

### pgdump 실패 :2025-02-07:
+ [[diary:2025-02-07]]
- backup 이 nas 속도 이슈로 fail, cronjob 에서 deadline 제거
- [[serverless]] 로부터 이전하며 변경된 로직 오류
- [X] 기존 정상적인 채널이 ban 된 것 찾아 복구함
  - [X] 5005 개 채널 복구됨

## 배포
- [[diary:2025-02-06]]
  + https://github.com/deptno/salji.ro/pull/647
  - ui 업데이트
    - 사이버펑크 테마 적용
  - salji 와 중복되는 기능 모두 제거, 커뮤니티 패널만 남겨둠

## link
- [[project]]
- [[salji.ro]]
- [[wn.private:youtube-data-api]]
