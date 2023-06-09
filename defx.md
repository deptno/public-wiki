# defx

https://github.com/Shougo/defx.nvim

## 설치 후
```vim
:UpdateRemotePlugins
```

## 사용
`sf`

## [[error]]
[[nvim-treesitter]] 설치 후 defx 에 접근시 에러가 발생

```vim
[defx] Vim(let):Error invoking '/Users/deptno/.local/share/nvim/site/p
ack/_undefined/start/defx.nvim/rplugin/python3/defx:function:_defx_ini
t' on channel 5 (python3-rplugin-host):^@no request handler registered
 for "/Users/deptno/.local/share/nvim/site/pack/_undefined/start/defx.
nvim/rplugin/python3/defx:function:_defx_init"
[defx] function defx#util#call_defx[2]..defx#start[10]..defx#initializ
e[1]..defx#init#_initialize[5]..defx#init#_channel[26].._defx_init[1].
.remote#define#request, line 2
[defx] defx failed to load. Try the :UpdateRemotePlugins command and r
estart Neovim. See also :checkhealth.
```

아래 명령어로 해결 가능
```vim
:UpdateRemotePlugins
```

## link
- [[neovim]]
- [[nvim-treesitter]]
