# tree-sitter

> 고성능 언어 파서

- 기존 syntax highlighting 이 regexp 에 의존, 부정하고 느렸던 것을 더 정확하게 파악한다
- [ctags](ctags) 와 유사한  느낌으로 다양한  언어를 문맥적으로도 해석가능하므로 [lsp](lsp) 와 같은 곳에서 쓰일 수도 있다

## syntax

### 코드 컨셉

#### an example program
- parser 생성
- parser += language
- tree = parser + code
- tree 에서 root_node 생성

#### multi-language documents
- 사용중에 parser +=  language 
- parser += language_range
- tree = parser + code
- tree 에서 root_node 생성

#### other tree operations 
- 효과적으로 트리를 투어하는 방법은 cursor 를 사용하는 것
- cursor 는 stateful 오브젝트
- node 에서 cursor 생성
- cursor 관련 함수는 동작이 성공하면 `true`, 동작할 수 없으면 `false` 를 리턴
- cursor 를 통해 항상 현재 위치의 node 및 연관된 field

### 객체
- language
- parser - stateful
- tree - source code 와 1:1
- node - n:1 tree

### {concrete,abstract} syntax tree
- concrete syntax tree
  - named ndoe: 문법에서 명시적으로 이름이 제공된
    - named 인지 확인하는 함수가 있다 (c: ts_node_is_named)
- abstract syntax tree - 덜 중요한 부분들이 제거된 버전
  - anonymous node: 문법에서 string 으로 처리된
    - (c: ts_node_named_*) 함수와 같이 `_named_`  함수를 통해서 순환시 skip 가능하다

### query
- `_` wildcard
- `.` anchor
  - `(node . (child))` first match
  - `(node (child) .)` last match
  - `(node (child) . (child))` 연속된 sibling 만 match i.g. a,b,c,d: (a,b),(b,c),(c,d)
  - `string` literal 과는 **사용 불가**
- `@` capture 재사용을 위해 이름을 부여한다
- `#` predicate @capture 에 대해 조건을 부여한다 @capture 구문 종료 후 사용
```
((language) @lang (#eq? @lang "lua"))
```

### static type node
- 타입언어에 관해서는 `node-types.json` 타입 파일이 생성되고 이를 활용할 수 있다 정도로 이해

## code navigation

### tag
- @role.kind
- @name
- @doc

| Category                 | Tag                       |
| ------------------------ | ------------------------- |
| Class definitions        | @definition.class         |
| Function definitions     | @definition.function      |
| Interface definitions    | @definition.interface     |
| Method definitions       | @definition.method        |
| Module definitions       | @definition.module        |
| Function/method calls    | @reference.call           |
| Class reference          | @reference.class          |
| Interface implementation | @reference.implementation |

#### built-in function
```tree-sitter
#strip! @doc "^#\\s*"
#select-adjacent! @doc @definition.class
```
- #select-adjacent! @capture-of-docstring @adjacent-capture
- #strip! @capture-of-docstring, "regular expression"

## [nvim treesitter](nvim-treesitter)
- [neovim](neovim) 에서 사용하는 tree-sitter

## test
+ https://thevaluable.dev/tree-sitter-neovim-overview/

```lua
local ts = require 'nvim-treesitter'
local ts_utils = require 'nvim-treesitter.ts_utils'
print(ts_utils.get_node_at_cursor())
```
---

## link
- [ctags](ctags)
- [lsp](lsp)
- [[treesitter]]
