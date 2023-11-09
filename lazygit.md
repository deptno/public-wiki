# lazygit

> tui git client

+ [config 설정](https://github.com/jesseduffield/lazygit/blob/master/docs/Config.md)
+ [내 설정:2023-10-26](https://github.com/deptno/.config/blob/5789b6fef0075ee1104f45952906afe73ce99521/.config/lazygit/config.yml)

## [@todo](@todo)
- [X] custom command
  - [X] commits
    - [X] filter by author + https://github.com/deptno/.config/commit/24db073d9781906b271b31e10a15f833472ed048
  - [X] [gh](gh) 와 연동해서 브랜치 PR 열기
    + https://github.com/deptno/.config/commit/a7216f3dc82dc50d7e42721568986a84ee6b3daa

## color 구분
> green yellow red 등 색상은 테마에 따라 달라지니 참고

- commit 들이 green 으로 표현
- [X] ~~main~~ 현재 브랜치(git.mainBranches 옵션에 의해 지정된)에 포함되지 않은 커밋은 yellow 로 표현되는 것으로 *추정*된다
- red 로 표시되는 커밋들은 아직 remote 에 push 되지 않은 커밋

## custom command
`:` 로 트리거 가능
`config.yml` 파일에서 생성하여 사용가능

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
