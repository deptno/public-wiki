# telescope.nvim

## development
+ https://github.com/nvim-telescope/telescope.nvim/blob/master/developers.md

### extension
> extension 형태로 구현을 하게되면 `ex` 명령형태로 사용 가능하다.

```vim 
:Telescope [extension] [stuff]
```

- 구현을 위해서는 아래와 같은 폴더 구조를 사용한다
```sh 
.
└── lua
    ├── plugin_name             # Your actual plugin code
    │   ├── init.lua
    │   └── some_file.lua
    └── telescope
        └── _extensions         # The underscore is significant
            └─ plugin_name.lua  # Init and register your extension
```

- `plugin_name.lua` 에 들어가는 내용은 아래와 같다
```lua
return require("telescope").register_extension {
  setup = function(ext_config, config)
    -- access extension config and user config
  end,
  exports = {
    stuff = require("plugin_name").stuff
  },
}
```

## link
- [[neovim]]
