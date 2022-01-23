# error

## vimwiki gk 로 발생
- [[neovim]]
- [[gitsigns]]
```sh
Error executing vim.schedule lua callback: ...k/packer/start/gitsigns.nvim/lua/gitsigns/subprocess.lua:71: Failed to spawn process: {
  _state = {
    pid = "EMFILE: too many open files",
    stderr = <userdata 1>,
    stderr_data = {},
    stdout = <userdata 2>,
    stdout_data = {}
  },
  args = { "--no-pager", "--git-dir=/path/to/wiki/.git", "ls-files", "--stage", "--others", "--exclude-standard", "--eol", "/Users/dept
no/workspace/src/github.com/deptno/wiki/file.md" },
  command = "git",
  cwd = "/path/to/wiki",
  supress_stderr = true
}
stack traceback:
        ...k/packer/start/gitsigns.nvim/lua/gitsigns/subprocess.lua:71: in function 'run_job'
        ...ite/pack/packer/start/gitsigns.nvim/lua/gitsigns/git.lua:160: in function 'returned_function'
        ...ck/packer/start/plenary.nvim/lua/plenary/async/async.lua:26: in function 'callback_or_next'
        ...ck/packer/start/plenary.nvim/lua/plenary/async/async.lua:40: in function <...ck/packer/start/plenary.nvim/lua/plenary/async/async.lua:39>
        [C]: in function 'wait'
        ...w/Cellar/neovim/0.6.1/share/nvim/runtime/lua/vim/lsp.lua:1371: in function '_vim_exit_handler'
        [string ":lua"]:1: in main chunk
stack traceback:
        [C]: in function 'error'
        ...k/packer/start/gitsigns.nvim/lua/gitsigns/subprocess.lua:71: in function 'run_job'
        ...ite/pack/packer/start/gitsigns.nvim/lua/gitsigns/git.lua:160: in function 'returned_function'
        ...ck/packer/start/plenary.nvim/lua/plenary/async/async.lua:26: in function 'callback_or_next'
        ...ck/packer/start/plenary.nvim/lua/plenary/async/async.lua:40: in function <...ck/packer/start/plenary.nvim/lua/plenary/async/async.lua:39>
        [C]: in function 'wait'
        ...w/Cellar/neovim/0.6.1/share/nvim/runtime/lua/vim/lsp.lua:1371: in function '_vim_exit_handler'
        [string ":lua"]:1: in main chunk

```
