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

## too many open files
+ https://github.com/grafana/loki/issues/1153
- [[kubernetes]]
- [[promtail]]
```sh
level=error ts=2023-01-23T18:00:42.109504938Z caller=main.go:167 msg="error creating promtail" error="failed to make file target manager: too many open files"
```
  - [ ] host
    - process 수정
      process에 ulimit 설정으로 해결되지 않는 것으로 보임
```sh 
$ ps -ef | grep promtail
deptno   4048974 4046594  0 17:47 pts/2    00:00:00 grep --color=auto promtail
$ prlimit --nofile --output RESOURCE,SOFT,HARD --pid 4046594
RESOURCE SOFT    HARD
NOFILE   1024 1048576
$ prlimit --nofile=4096 --pid 4046594
$ prlimit --nofile --output RESOURCE,SOFT,HARD --pid 4046594
RESOURCE SOFT HARD
NOFILE   4096 4096
```
    - runtime 수정(docker, containerd 설정 /etc/docker/datmon.json, containerd 는 없는 것으로 보임)
    - /etc/security/limits.conf 수정 -> 리붓, 적용을 안해봐서 파일 수정만으로는 일단 안되는 것 확인
  - [X] container
    initContainer 로 fs.inotify.max_user_instances 값을 128->512 변경으로 성공
```yaml
initContainer:
 - name: init
   image: docker.io/busybox:1.33
   command:
     - sh
     - -c
     - sysctl -w fs.inotify.max_user_instances=512
   securityContext:
     privileged: true
```
