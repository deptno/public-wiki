# git-log

[[@todo]] - [[git]] 에서 데이터 옮겨오기

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

## link
- [[git]]
- [[.gitconfig]]
