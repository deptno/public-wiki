# psql

## 환경 설정
- `~/.pgpass` 
  - database, user 에 따른 비번 설정
- `~/.psqlrc`
  - 접속후 psql 내에서의 환경 설정

## command line option
- U username
- d database
- c 실행할 sql

## 접속후
- `\l` database 목록
- `\c` database 선택
- `\d table` 테이블 구조 확인
- `\t` 테이블 목록
- `\dt` 테이블 구조 확인
- `\q` 나가기
- `\dn+ schema`
- 쿼리는 여러줄이 가능하면 엔터로 끝나는게 아니라 `;` 로 끝나야 동작

## link
- [[postgresql]]
- [[sql]]
