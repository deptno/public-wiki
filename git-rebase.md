# git-rebase

## 커밋 해시가 변경되지 않는 경우
rebase -i @~n 을 통해 변경시에 커밋 메시지만을 변경하면 commit hash 는 변경되지 않는다

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

## link
- [[git]]

