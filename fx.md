# fx

+ https://fx.wtf/getting-started

[[jq]] 의 interactive 버전

## usage

### cli
[[javascript]] 문법을 통해서 제어가 가능하다

```sh 
echo '{"name": "world"}' | fx 'x => x.name' 'x => `Hello, ${x}!`' # javascript 함수 지원
echo '{"text": "Hello"}' '{"text": "World!"}' | fx .text # JsonLD 지원
```

#### syntactical sugar
```sh 
echo '{"name": "world"}' | fx .name '`Hello, ${x}!`' # syntactical sugar 1. `.name` , 함수 body만 사용하고 인자가 `x` 인걸 가정
```

- `.[property name]` 을 통해서 단순하게 접근
- 단항 함수인자를 `x` 로 가정하고 함수 body 부분만 작성해 진행 가능

#### multiple lines -> single line
```sh 
echo '{"text": "Hello"}' '{"text": "World!"}' | fx --slurp '.map(x => x.text)' '.join(", ")'
```
`--slurp` 혹은 `-s` 옵션을 통해서 여러 라인은 할줄로 출력

### interactive mode
- `.` 키를 fuzzy search 를 지원한다

## custom
- `.fxrc.js` 를 통한 함수 확장도 지원한다
  - [ ] 문서에 디렉토리가 안나와있는데 테스트를 해봐야한다 [[@todo]]

## tip
### unicode
+ https://github.com/antonmedv/fx/issues/271
```sh
[json] | fx . | fx
```
`fx .` 를 통해서 한번 evaluation 을 하고 진입하면된다

## link
- [[jq]]
- [[curl]]
