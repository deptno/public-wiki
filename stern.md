# stern

여러 파드의 로그를 함께 볼 수 있다.
cronjob 등에 의해 새로 생성되는 파드도 함께 보는 것이 가능하다

## install
```sh
$ k krew install stern
```

## usage
```sh
kubectl stern [optinos] [name pattern]
kubectl stern -o raw [name pattern] | npx pino-pretty
```
raw 포맷을 지원하기 때문에 [[pino]] 와 같이 prettier 가 별도로 존재하는 로깅 라이브리를 쓴다면 편리하게 이용할 수 있다

### options
| option    | description                            |
|-----------|----------------------------------------|
| -o [type] | output 을 지정한다 대표적인 타입은 raw |
| -t        | timestamp 를 표시한다                  |

## link
- [[kubernetes]]
- [[kubetail]]
