# git-log

[[todo]] - [[git]] 에서 데이터 옮겨오기

## options
- --all
- --date-order
- --author
- --ancestry-path [[sha1..sha2]]

## usage
```sh
git log [sha1]^..[sha1] --first-parent | tail -1 # 최초 포함된 머지커밋
git log [sha1]^..[sha1] --first-parent --merges  # 트리
git log [sha1]^..[sha1] --first-parent --merges --simplify-by-decoration # 브랜치 포함 순서 트리
```

### 커밋 메시지 검색
```sh
# git log --grep=[REGEXP]
git log --grep [ pattern ]
git log --grep [ pattern ] -i # case insensitive 
git log --grep=aaaaaaaaa

commit bbbbbbbbb (origin/feature/aaa)
Author: b
Date:   xx
Revert "hello world"

    This reverts commit aaaaaaaaa

commit aaaaaaaaa (fix/a)
Author: a
Date:   yy

    hello world
```
- ~~커밋 SHA로 검색을 하게 되면 **정상적인 리버트 커밋이 있는 경우** 함께 검색된다.~~
  - 위에 내용은 잘못된 내용으로 보이고 `git log --grep` 은 메시지 내용만을 검색한다
  - 그러나 Revert 커밋은 커밋 메시지 안에 원본에 대한 커밋값을 가지고 있기때문에 해당 내용을 참조할 수 있다

## link
- [[git]]
- [[git-grep]]
- [[.gitconfig]]
