# neovim

## list
- buflist `:buffers`
- arglist `:args`
  - buffer list 의 subset 으로 argument 로 열린 리스트가 보여진다
  - argadd 명령을 통해서 확장 가능하다

## ex
- `@:` 최근 명령어 실행

## [[python]]
```sh
# python 지원 확인
nvim +checkhealth
# python 지원 업그레이드
python3 -m pip install --upgrade pynvim
```

## [[error]]
emoji 등 유니코드 캐릭터가 존재한 이후에는 yy 등 카피가 clipboard(reg *) 에 복사되지 않는 이슈

- https://github.com/neovim/neovim/issues/11432#issuecomment-557868656
```lua
vim.cmd ":let $LANG='en_US.UTF-8'"
```

### .local/share/nvim/runtime/lua/vim/lsp/semantic_tokens.lua:98: attempt to index field 'semanticTokensProvider' (a nil value)
+ https://github.com/neovim/nvim-lspconfig/issues/2542#issuecomment-1547019213
- 다소 다르게 처리하긴 했는데 `lsp-config` 에서 `semanticTokensProvider` 관련 설정을 `on_init` 으로 타이밍 변경후 나지 않는다
  + https://github.com/deptno/nvim/commit/bc8273d6600f3f1964b1abb0136dc8c577f6000c
  - 무엇이 안되는지 봐야겟으나 `dynamic symbols` 은 동작하는 것으로 보인다
    - `rename` 이 안되는 것으로 보임
```vim 
Error on prepareRename: Request textDocument/prepareRename failed with message:  You cannot rename this element. 
```

### [after/]ftplugin/*.lua 에서 설정한 키맵이 전역으로 번지는 문제
> 예를 들어 `markdown` filetype 에 특정키 `q` 를 매핑해서 `bdelete` 를 하게되면 `markdown` 이후의 모든 파일들에대해서 해당 키맵이 동작하는 문제
- filetype 을 통해서는 해결이 안되는 것으로 보인다. 권장하지 않는 방법이라는 글들이 있다.
- 그냥 글로벌 키매핑으로 빼버렸다
  + https://github.com/deptno/nvim/commit/ef0ffc616b0a7971936e32c83d5d6b42934261e7

## plugin
- [[taskwiki]]
- [[defx]]
- [[nvim-web-devicons]]
- [[nvim-treesitter]]
- [[telescope|telescope.nvim]]
- [[gitsigns|gitsings.nvim]]
- [[bufferline|bufferline.nvim]]
- [[null-ls|null-ls.nvim]]
- [[nlspsettings|nlspsettings.nvim]]

### 북마크
- bookmarks.nvim
  + https://github.com/deptno/nvim/commit/ed11001b34706b8f079c0e19df21545c59d813c4
  - [[git]] 프로젝트 기반으로 영구적인 북마크를 생성한다, 리스팅과 태그 프리뷰가 지원
- marks.nvim
  + https://github.com/deptno/nvim/blob/ed11001b34706b8f079c0e19df21545c59d813c4/lua/custom/configs/marks.lua
  - custom mark 를 지원하며 동일 북마크키로 여러군대를 지정하고 네비게이션이 가능하다
  - 영구적이지않아 이슈 분석등을 일시적으로 할때 편리하다

### neovim lua 기반 플러그인 환경 설정
- [[neovim-lua-plugins]]

---

## neovim-api
- global events
  - client async request -> error occur 
  - server -..-> notify error event later

### buf -> stdin -> stdout
```lua
local buf = vim.fn.getbufline('%', 1, '$')
local stdin = buf.fn.getbufline('%', 1, '$')
vim.fn.system('grep content', stdin)
```
`vim.fn.system` 의 두번째 인자가 `stdin` 의 역할을 한다

---
## tabpage > window > buffer [[@todo]]
- [[nvchad]] 는 [[tabufline]] 을 사용
- 창 분할마다를 window
- buffer 는 window 아래로 귀속되지 않는 것으로 보인다
### tabpage
- `vim.api.nvim_tabpage_get_number(0)` -> 현재 tabpage number (우상단 표시)
- `vim.api.nvim_tabpage_get_win(0)` -> 현재 tabpage number (우상단 표시)
  - [[question]] `nvim_tabpage_get_number(0)` 을 통해 넣은 경우(현재윈도우)와 `0` 을 넣은 경우가 다름
#### window
- `vim.api.nvim_win_get_tabpage(0)` -> 현재 window 의 tab nubmer
- `vim.api.nvim_win_get_buf(0)` -> 현재 window의 buf number

## link
- [[vim]]
- [[python]]
- [[neovim-lua-plugins]]
- [[neovim-setup]]
