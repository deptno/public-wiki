# neovim-lua-plugins

## 구조
- [[path|~/.config/nvim/init.lua]]
- [[path|~/.local/share/nvim/site/pack/*/start]] - 시작시 로딩
- [[path|~/.local/share/nvim/site/pack/*/opt]] - 원하는 타이밍에 스크립트를 통해서 로딩

## lua 전환
### 시작 설정 옮기기 전
```sh
Permissions Links Size User   Date Modified    Git Name
drwxr-xr-x      3    - deptno 2022-01-10 13:02  -N  .
drwxr-xr-x     31    - deptno 2022-01-12 18:48  -N └──  start
drwxr-xr-x     15    - deptno 2022-01-12 18:48  -N    ├──  vim-rest-console
drwxr-xr-x     10    - deptno 2022-01-12 01:16  -N    ├──  goyo.vim
drwxr-xr-x      9    - deptno 2022-01-12 01:16  -N    ├──  limelight.vim
drwxr-xr-x      8    - deptno 2022-01-11 21:56  -N    ├──  vim-unimpaired
drwxr-xr-x     13    - deptno 2022-01-11 21:54  -N    ├──  bufferline.nvim
drwxr-xr-x     15    - deptno 2022-01-11 19:34  -N    ├──  null-ls.nvim
drwxr-xr-x     21    - deptno 2022-01-11 19:14  -N    ├──  gitsigns.nvim
drwxr-xr-x     20    - deptno 2022-01-10 23:08  -N    ├──  plenary.nvim
drwxr-xr-x     21    - deptno 2022-01-10 23:03  -N    ├──  telescope.nvim
drwxr-xr-x     25    - deptno 2022-01-10 17:57  -N    ├──  nvim-treesitter
drwxr-xr-x     13    - deptno 2022-01-10 14:25  -N    ├──  copilot.vim
drwxr-xr-x      6    - deptno 2022-01-06 21:04  -N    ├──  vim-autoswap
drwxr-xr-x     13    - deptno 2022-01-06 20:56  -N    ├──  vim
drwxr-xr-x      7    - deptno 2022-01-06 13:43  -N    ├──  defx-icons
drwxr-xr-x      7    - deptno 2022-01-06 13:42  -N    ├──  defx-git
drwxr-xr-x     13    - deptno 2022-01-06 13:42  -N    ├──  defx.nvim
drwxr-xr-x      6    - deptno 2022-01-05 02:47  -N    ├──  nvim-web-devicons
drwxr-xr-x      6    - deptno 2022-01-05 02:46  -N    ├──  nvim-nonicons
drwxr-xr-x     10    - deptno 2022-01-05 02:46  -N    ├──  galaxyline.nvim
drwxr-xr-x      8    - deptno 2022-01-05 02:43  -N    ├──  vim-surround
drwxr-xr-x     12    - deptno 2022-01-05 02:43  -N    ├──  vim-rhubarb
drwxr-xr-x     14    - deptno 2022-01-05 02:42  -N    ├──  vim-fugitive
drwxr-xr-x     17    - deptno 2022-01-05 02:19  -N    ├──  nvim-cmp
drwxr-xr-x      8    - deptno 2022-01-05 02:18  -N    ├──  rust-tools.nvim
drwxr-xr-x     23    - deptno 2022-01-05 02:17  -N    ├──  rust.vim
drwxr-xr-x     24    - deptno 2022-01-05 02:17  -N    ├──  nvim-lspconfig
drwxr-xr-x     14    - deptno 2022-01-05 02:16  -N    ├──  tagbar
drwxr-xr-x     14    - deptno 2022-01-05 02:16  -N    ├──  vim-startify
drwxr-xr-x     17    - deptno 2022-01-05 02:15  -N    └──  vimwiki
```

### 전환 체크
- [X] vim-rest-console
  - [X] NTBBloodbath/rest.nvim.git
- [ ] goyo.vim
- [ ] limelight.vim
- [ ] vim-unimpaired
  - 필요성을 아직 못느낌
- [ ] bufferline.nvim
  - 써보니 일반 buffer 는 일반 에디터의 tab과 매치되지 않아 혼란 tab === tab
- [X] null-ls.nvim
- [X] gitsigns.nvim
- [X] plenary.nvim
- [X] telescope.nvim
- [X] nvim-treesitter
- [X] copilot.vim
- [ ] vim-autoswap
  - `vim.opt.swapfile` off 인 인하여 현재 필요 없다고 판단
- [X] vim (dracula theme)
- [.] defx.nvim
  - [ ] defx-icons
  - [ ] defx-git
  - [X] nvim-tree 로 전환, python 종속성 에러가 너무 많이 남
- [X] nvim-web-devicons
- [X] nvim-nonicons
- [X] galaxyline.nvim
  - [X] lualine 으로 전환, 개발 비활성화 deprecated 된 api 호출로 워닝
- [X] vim-surround
- [X] vim-rhubarb
- [X] vim-fugitive
- [X] nvim-cmp
- [X] rust-tools.nvim
- [ ] rust.vim
- [X] nvim-lspconfig
- [X] tagbar
- [X] vim-startify
- [X] vimwiki
- [X] nvim-orgmode

## link
- [[setup-terminal|맥 터미널 설정]]
- [[orgmode]]
