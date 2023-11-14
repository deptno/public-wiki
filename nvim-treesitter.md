# nvim-treesitter

## usage
- [[markdown]] 기반인 [[vimwiki]] filetype 의 문법 지원
```lua
-- markdown -> vimwiki filetype 에 적용
vim.treesitter.language.register('markdown', 'vimwiki')
```

## treesitter-playground

### lazy 설정
+ https://github.com/deptno/nvim/commit/62e43094797d94ab27e5008d87ea56ec51072a4d

## [[error]] [[m1]]
```vim
Error detected while processing FileType Autocommands for "*":
E5108: Error executing lua Failed to load parser: uv_dlopen: dlopen(/U
sers/deptno/.config/nvim/pack/_undefined/start/nvim-treesitter/parser/
vim.so, 0x0001): tried: '/Users/deptno/.config/nvim/pack/_undefined/st
art/nvim-treesitter/parser/vim.so' (mach-o file, but is an incompatibl
e architecture (have 'x86_64', need 'arm64e')), '/usr/local/lib/vim.so
' (no such file), '/usr/lib/vim.so' (no such file)
stack traceback:
        [C]: in function '_ts_add_language'
        ...0.6.1/share/nvim/runtime/lua/vim/treesitter/language.lua:33
: in function 'require_language'
        ...r/neovim/0.6.1/share/nvim/runtime/lua/vim/treesitter.lua:38
: in function '_create_parser'
        ...r/neovim/0.6.1/share/nvim/runtime/lua/vim/treesitter.lua:93
: in function 'get_parser'
        .../start/nvim-treesitter/lua/nvim-treesitter/highlight.lua:10 7: in function 'attach'
        ...ed/start/nvim-treesitter/lua/nvim-treesitter/configs.lua:45
8: in function 'attach_module'
        ...ed/start/nvim-treesitter/lua/nvim-treesitter/configs.lua:48
1: in function 'reattach_module'
        [string ":lua"]:1: in main chunk
```

```sh
file /Applications/Alacritty.app/Contents/MacOS/alacritty
/Applications/Alacritty.app/Contents/MacOS/alacritty: Mach-O 64-bit executable x86_64
```
- [[alacritty]]@0.9 [[x86_64]]
검색 결과로는 [[arm64]] native 를 사용하는 경우 이슈가 `없는 것` 으로 보인다.
`:TSInstall` 시에 설치되는 [[so]] 파일들의 빌드가 문제가 된다.

[[arm64]] 기반의 쉘에서 설치가 되어야한다. [[uname]] -m 을 통해서 아키텍쳐 확인이 가능하다.
[[arm64]] 버전의 터미널에서 실행하면 결과가 `arm64` 로 확인된다.
맥에 기본으로 포함된 터미널에서 실행하면된다.

```sh
uname -m
arm64
```
확인 후에는 [[neovim]] 실행 후에 아래와 같이 명령어를 입력한다.
```vim
:TSUninstall all
:TSInstall all
```
해당 파일들을 열어보면 파로 에러가 제거된 것을 확인할 수 있다.

## link
- [[neovim]]
- [[telescope]]
- [[defx]]
- [[m1]]
- [[alacritty]]
- [[tree-sitter]]
