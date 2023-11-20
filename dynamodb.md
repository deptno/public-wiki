# dynamodb
+ https://googit.io/post/ap-northeast-2:c03f8bf0-992e-48a8-93b6-15787a0fc96f/public/dynamodb/
+ https://github.com/deptno/yiguana - dynamodb 기반의 게시판 sdk
+ https://github.com/deptno/dynalee - dynamodb 기반의 orm

> 사용한지 5년정도가 지나 정리하는 것이 때문에 기억이 온전치 않다
> 개념만 확인하자

## 요약
- 1 table === 1 application, model 이 아닌 application 과 매칭된다
- 테이블 생성시 `hash key` 혹은 `hash key` + `range key` 로 unique key 조합을 선정
  - 아이템 입력시에는 반드시 `unique key` 가 존재해야한다
- secondary index
  - global secondary index
    - table `hash key`, `range key` 에 대해서 모두 다른 컬럼을 선택해서 새롭게 인덱스를 생성한다
  - local secondary index
    - table `hash key` 는 그대로 사용하고 `range key` 만 다른 컬럼을 사용해서 인덱스를 생성한다
  - index 로 사용되지 않을 컬럼은 schema 로 존재할 필요가 없으며 그냥 json 등의 컬럼에 넣어버리면된다
    - 검색용 컬럼이 분리된다는말
    - 심지어 검색용 컬럼이 아닌 그냥 데이터는 바이너리로 압축해서 넣어도된다
- scan 은 full scan  을 의미한다
- query 는 인덱스를 선택하여 질의가 가능하며 몇가지 operator 가 제공된다
- ttl 이 지원되어 특정 시점에 삭제를와 이벤트를 발생시켜 처리가 가능하다

## link
- [[db]]
