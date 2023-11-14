# develop neovim lua plugin|루아 플러그인 개발

로컬에 대충 짜놓은 코드들이  관리하기 힘들게 늘어나고 import 구문이 복잡해서 처리하기 복잡하다

## [[@todo]]
1. [ ] luarocks 등의 패키지 매니저를 통해 라이브러리 설치시 최종 배포가 어떻게 되는지
  - 번들링 되어야하는 것으로 보고 있다 `plenary` 참조
  - luarocks 라이브러리가 neovim 의 plugin dependency 처럼 인식되어 처리될 수 있을 것으로 생각, 예를 들어
    ```lua
    {
      'deptno/plugin.nvim',
      dependencies = {
        'luarocks/dependency.example',
      },
    }
    ```
- [ ] 테스트 환경 구축
  - defacto 로 요구되는 `nvim-lua/plenary.nvim` 패키지에 관련 함수들이 패키징 된것으로 보임(`luaasert`)
    + https://github.com/nvim-lua/plenary.nvim/blob/master/TESTS_README.md
  - nvim -l
- [X] runtimepath 적용 + vim, lua 모듈 환경 이해
  - [X] 루아 프로그래밍 3E + 15장 모듈과 패키지 179-192
  - [X] `runtimepath` `require` 구문에 의해서 사용되는 `path` 를 추가하는 과정으로 인식
  - [X] `runetime` - 해당 패스를 source 하는 명령어
- [X] code style 적용
  - [X] .stylua.toml +https://github.com/deptno/nvim/commit/441ecb4d8faf4c20285f47d5647889d312aa7d90
```vim
set rtp+=.
runtime plugin/plenary.vim
```
- [ ] 개발 환경 설정

## 관련 파일들
- .luacheckrc
- .stylua.toml
- .styluaignore
- rockspec.template

## link
- [[lua]]
- [[neovim]]
