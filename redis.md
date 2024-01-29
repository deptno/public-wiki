# redis

## 설치
- [[helm]] 을 통해서 기본값 설치 다음 에러와 함께 죽어있다. pvc 를 확인해보면 `PENDING` 상태
  ```sh 
  no persistent volumes available for this claim and no storage class is set
  ```
- `slave` 가 필요없어서 `replicaCount: 0`
- 개발 환경에서는 `persistence: false`
- 프로덕션 환경에서는 pvc, pv 생성 후 `existingClaim` 에 이름 명시

## 사용
```sh
redis-cli -h 127.0.0.1 -p 6379
```
- 로컬에 설치 할 일은 없어서 `redis-cli` 가 없다. 해당 쉘에 접속해서 테스트한다
  + https://redis.com/blog/get-redis-cli-without-installing-redis-server/

```sh 
$ redis-cli INCR mycounter # mycounter 1 증가
$ redis-cli -a [AUTH] [COMMAND] # 인증
$ redis-cli -n [DB_NO] [COMMAND] # 샤딩이 있을 때 선택하는 것인가 하고 추측
$ redis-cli -h [HOST] -p [PORT]
$ redis-cli -u [HOST_URI]
$ redis-cli -r [REPEAT_COUNT] [COMMAND] # 샤딩이 있을 때 선택하는 것인가 하고 추측
$ redis-cli -r -1 -i 1 INFO | grep rss_human # 무한 반복
$ cat /tmp/commands.txt | redis-cli # stdin 

# 리스트 인풋, csv 아웃풋
$ redis-cli LPUSH mylist a b c d
# (integer) 4
$ redis-cli --csv LRANGE mylist 0 -1
# "d","c","b","a"

# 루아 스크립트 실행
$ cat /tmp/script.lua
# return redis.call('SET',KEYS[1],ARGV[1])
$ redis-cli --eval /tmp/script.lua location:hastings:temp , 23
# OK

$ redis-cli
SELECT [DATABASE_INDEX]
DBSIZE
5 INCR mycounter # repl 에서는 앞에 숫자를 붙여서 반
```

## 공식문서
[[tbd]] [[wip]]

### Data Structure store
```sh
# key value
> SET,GET, JSON.SET
# hashmap
> HSET,HGET
# 이미 존재하는 json 도 prefix 에 의해서 `bicycle:` 로 시작하는 키는 인덱싱
SCAN [0:START_INDEX] MATCH "key:*" COUNT [LIMIT:100] # 키 스캔
```

### Document database
```sh
# `bicycle:` 로 시작하는 키값은 자동으로 인덱스에 추가
> FT.CREATE idx:bicycle ON JSON PREFIX 1 bicycle: SCORE 1.0 SCHEMA $.brand AS brand TEXT WEIGHT 1.0 $.model AS model TEXT WEIGHT 1.0 $.description AS description TEXT WEIGHT 1.0 $.price AS price NUMERIC $.condition AS condition TAG SEPARATOR ,
# 검색
> FT.SEARCH "[INDEX_NAME]" "*" LIMIT 0 10
> FT.SEARCH "idx:bicycle" "@model:Jigger" LIMIT 0 10
# exact match
> FT.SEARCH "idx:bicycle" "@brand:\"Noka Bikes\"" LIMIT 0 10
```

### Vector database
- [[tbd]]

## link
- [[db]]
