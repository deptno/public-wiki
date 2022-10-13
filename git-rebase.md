# git-rebase

## error
```sh
$ git rebase --abort
warning: could not read '.git/rebase-merge/head-name': No such file or directory
$ git rebase --quit
$ git rebase --onto TAG @~ @ # 특정 태그 위치에 헤드 커밋 하나를 이동시킴, cherry pick 과 같음
$ git rebase --onto F D
#         Before                           After
#   A---B---C---F---G (branch)        A---B---C---F---G (branch)
#            \                                     \
#             D---E---H---I (HEAD)                  E---H---I (HEAD)
#
$ git rebase --onto F D H
#         Before                                     After
#   A---B---C---F---G (branch)                A---B---C---F---G (branch)
#            \                                             \
#             D---E---H---I (HEAD)                          E---H (HEAD)
```

## related
- [[git]]

