# lazygit

> tui git client

## custom command
`:` 로 트리거 가능

- 해당 커밋이 있는 브랜치 리스트 -> checkout
```sh 
pbpaste | xargs -0 git branch --contains | fzf --bind 'enter:become(git switch $(echo {} | tr -d "* "))' --header "contains $(pbpaste)"

```

## link
- [golang](golang)
- [git](git)
- [tig](tig)
- [fzf](fzf)
