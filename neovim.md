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

## link
- [[vim]]
- [[python]]
- [[neovim-lua-plugins]]
