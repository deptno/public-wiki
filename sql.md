# sql

## cursor - 무한 스크롤
1. limit + offset
  - 대규모 테이블에서 성능 이슈가 있을 수 있다.
  - 페이지네이션 구조에 적합
  - 무한스크롤 구현이 가능
    - **2** 를 쓸 수 없는 경우에 사용 적합
2. limit + order by COLUMN + where <  COLUMN
  - 무한 스크롤 구조에 적합
  - order by 에 사용되는 컬럼이 limit 이상으로 중복되는 경우 동작 이상이 발생한다
    - 예를 들어서 rank 로 정렬했는데 동일 rank 가 limit 개수 이상인 경우 무한 루프
3. cursor
  - 커서 기반의 컨트롤로 무한스크롤에 적합
  - [ ] 유저 세션별로 커서가 새로 생성되어야하는데 이 방법을 아직 찾아보지 않음

[[postgresql]] 에서 사용가능 한 옵션

## related
- [[postgresql]]
- [[psql]]
