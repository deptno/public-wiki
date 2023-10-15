# lua

lua 5.1 -> luajit -> 5.2

neovim 은 lua 5.1 을 스크립트 엔진으로 내장하고 있다. 추후에 neovim 에서 내장 스크립트 버전이 업데이트되는 경우 5.1 와의 호환성은 보장한다.

## cli
```sh 
lua # 인터프리터 시작
lua [FILENAME] # 파일을 실행
lua -i [FILENAME] # 인터프리터 시작과 함게 해당 파일을 불러와서 실행 `dofile` 과 같음
lua -e "LUA_CODE" # 코드 실행
lua -llib -e "LUA_CODE" # lib을 임포트한 후 `LUA_CODE` 실행
lua -llib -e -i "LUA_CODE" # lib을 임포트한 후 `LUA_CODE` 실행 후에 인터프리터 모드로 진입
```

### `arg`
`arg` 는 전역 배열로 `cli` 를 통해서 실행할때 스크립트 파일명을 기준으로 `0` 인덱스가 정해지고 그 이전은 `-` 인덱스가 된다
```
lua -e "print('hello')" script a b
```
- `arg[-3]` "lua"
- `arg[-2]` "-e"
- `arg[-1]` "print('hello')"
- `arg[ 0]` "script"
- `arg[ 1]` "a"
- `arg[ 2]` "b"

## 환경변수

### `INIT_LUA`
> 환경 변수의 내용에 따른 실행 분류
  - `@` 로 시작하는 경우에는 파일명으로 인지하고 해당 파일을 임포트
  - `@` 가 아닌 다른 것으로 시작하는 경우에는 루아 코드를 실행, `dofile` 과 같은 역할이라고 생각됨
- `INIT_LUA_5_2`
  - 버전명에따라서 suffix 가 달라지는데 5.1 이나 [[luajit]] 에서도 사용이 가능한지는 확인이 필요
- `INIT_LUA`
  - `INIT_LUA_5_2` 가 읽힌 후에도 읽어지는 확인 필요

## 문법

### import
```lua
dofile("FILENAME") -- eval
require("FILENAME") -- import
```

### 전역
`_CAPITAL` `_` 바로 시작되는 대문자 전역변수는 예약어로 사용

### 정규표현식
> lua 는 정규표현식을 fully 구현하지 않는다
`escape`
- 패턴 매칭 내에서도 이전 매칭 결과를 사용할 수 있음
  - `(['"])hello%1` `'` 혹은 `"` 이 매칭되었을때 `%1` 은 이전에 매칭된 결과와 같은 문자열로 픽스됨
    - 성공, `"hello"`, `'hello'`
    - 실패, `"hello'`, `'hello"`
- string pattern 매치에는 정규표현식의 `{7, 40}` 에 해당 하는 구문이 없다 [[diary:2023-10-12]]
  - git sha1 을 매칭하기 위해 사용했다.
  + https://github.com/deptno/NvChad/commit/6ed9f8a
```luajit
local is_git_hash = function(hash)
  local char = '^[0-9a-fA-F]$'
  local matched = string.match(hash, string.format('%s*', char:rep(8)))

  return matched ~= nil and #matched <= 40 and matched
end
```
형태로 구현했다. 7글자이상 40글자 이하일때 매칭된다, 링크된 코드는 line matching 을 가정해서 `^$` 가 제거된 버전이다

## vs javascript

### 타입확인
- lua: type(변수)
- js: typeof [변수]

### true/false
- lua: `false`, `nil` 만을 `false` 로 간주한다
- js: `undefined`, `null`, `""`, `0`, `NaN` 등 더 많은 상황을 `false` 로 간주한다

### regexp|정규표현식
- lua: `%` 를 escape 문자로 사용
- js: `\` 를 escape 문자로 사용

### lexical scoping
```javascript
function a() {
  v = 2
}
let v = 1

console.log(v) // 1
a()
console.log(v) // 2
```

```lua
function a()
  v = 2
end
local v = 1

print(v) // 1
a()
print(v) // 1
```
자바스크립트와는 다르게 클로저 스코핑이 조금 다르다
- `function a` 의 선언 시점이 `local v` 보다 이전이라 `local v` 에 접근할 수 없음
- 함수내에서 접근하려는 변수는 전역변수이거나 현재 함수의 **정의** 이전에 **선언**되어야한다.
  - `local a` 만 선언하고 `a = function {body} end` 등의 정의는 늦게 와도된다.
대충 스코핑이 함수 호출 시점이 아니라 정의 시점에 만들어진다고 생각하면 될 듯하다

## garbage collection
- `local` 은 메모리 할당을 생성하지 않지만 string 은 생성한다. 동일한 string 을 연속으로사용하는 것은 메모리를 낭비하지않는다.
- 가변인자 함수는 함수가 호출될때 인자 테이블을 생성한다
- `collectgarbage("count") * 1024` 로 확인

## coroutines
- coroutine.create 로 coroutine.yeild 반환시에 중단점을 저장하고 추후 coroutine.resume 을 통해 계속 실행할 수 있는 함수를 반환한다.
- coroutine.yeild 를 통해 반환 하는 시점이 다음 coroutine.resume 을 통해 인자를 전달하는 시점으로 이어진다

## 책
- [루아 프로그래밍 3판](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=43858872)
 
## doc
+ https://github.com/defold/doc/blob/master/docs/ko/manuals/lua.md
+ https://wariua.github.io/lua-manual/

## link
- [[neovim]]
