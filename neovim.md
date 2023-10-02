# neovim

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

### neovim lua 기반 플러그인 환경 설정
- [[neovim-lua-plugins]]

---

## neovim-api
- global events
  - client async request -> error occur 
  - server -..-> notify error event later

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
