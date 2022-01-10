# jobs

```
$ jobs                                                                                                                                   ok  11:18:52
$ bg                                                                                                                                     ok  12:06:15
bg: no current job
$ fg                                                                                                                                  1 err  12:06:16
fg: no current job
$ vim #suspend vim

[1]  + 86914 suspended  vim
$ jobs                                                                                                                              TSTP  %  12:06:22
[1]  + suspended  vim
$ bg                                                                                                                                 ok | %  12:06:24
[1]  + 86914 continued  vim
[1]  + 86914 suspended (tty output)  vim
$ fg # vim reopen
```

## related
- [[fg]]
- [[bg]]
