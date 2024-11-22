# jobs

```sh
$ jobs
$ bg
bg: no current job
$ fg
fg: no current job
$ vim #suspend vim

[1]  + 86914 suspended  vim
$ jobs
[1]  + suspended  vim
$ bg
[1]  + 86914 continued  vim
[1]  + 86914 suspended (tty output)  vim
$ fg # vim reopen
```

```sh
bg %1 # 백그라운드에서 1을 실행한다. suspended -> running
```

## link
- [[fg]]
- [[bg]]
