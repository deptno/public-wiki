# lazygit

> tui git client

+ [config 설정](https://github.com/jesseduffield/lazygit/blob/master/docs/Config.md)
+ [내 설정:2023-10-26](https://github.com/deptno/.config/blob/5789b6fef0075ee1104f45952906afe73ce99521/.config/lazygit/config.yml)


## color 구분
- commit 들이 green 으로 표현
- [ ] main 브랜치(git.mainBranches 옵션에 의해 지정된)에 포함되지 않은 커밋은 yellow 로 표현되는 것으로 *추정*된다
- red 로 표시되는 커밋들은 아직 remote 에 push 되지 않은 커밋

## custom command
`:` 로 트리거 가능

### 해당 커밋이 있는 브랜치 리스트 -> checkout
```sh 
pbpaste | xargs -0 git branch --contains | fzf --bind 'enter:become(git switch $(echo {} | tr -d "* "))' --header "contains $(pbpaste)"
```
- commit 뷰에서 `y`, `<c-y>` 를 통해 해시 복사가 후 해당 커밋이 있는 브랜치 확인이 가능

## link
- [golang](golang)
- [git](git)
- [tig](tig)
- [fzf](fzf)
