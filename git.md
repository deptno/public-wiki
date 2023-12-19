# Git

분산저장관리
공식문서: https://git-scm.com/book/ko/v2

## by-command
- [[git-apply]]
- [[git-add]]
- [[git-branch]]
- [[git-commit]]
- [[git-check-ignore]]
- [[git-checkout]]
- [[git-cherry-pick]]
- [[git-clean]]
- [[git-clone]]
- [[git-config]]
- [[git-diff]]
- [[git-difftool]]
- [[git-fetch]]
- [[git-grep]]
- [[git-log]]
- [[git-pull]]
- [[git-rebase]]
- [[git-reset]]
- [[git-merge-base]]
- [[git-restore]]
- [[git-show]]
- [[git-sparse-checkout]]
- [[git-stash]]
- [[git-status]]
- [[git-subtree]]
- [[git-submodule]]
- [[git-tag]]
- [[git-bisect]]
- [[git-credential-manager]]
- [[git-filter-branch]]
- [[git-filter-repo]]
- [[git-bundle]]
- [[git-unbundle]]

## files
- [[path|.gitconfig]]

## 특정 커밋 내용 가져와서 적용
  - https://stackoverflow.com/questions/5717026/how-to-git-cherry-pick-only-changes-to-certain-files
  ```sh
   git show [SHA] -- [FILE1] [FILE2] | git apply -
  ```
  ```sh
   git cherry-pink -n [SHA]
  ```

## 특정 커밋 내용 확인
  ```sh
  git show [SHA]:[PATH]
  ```

##  특정 커밋 내용 비교
  ```sh
  git diff [SHA0] [SHA1] [PATH]
  ```

## 깃에서 관리하는 파일인지 확인
```sh
git ls-files [FILE]
```

## 깃에서 무시하는 파일인지 확인
```sh
git check-ignore -v [FILE]
```
```sh
# list
git status --ignored [FILE]
```

## rebase -i
```sh
git log branch/a branch/b
* (3시간 전) c003c2c
* (3시간 전) c338742
* (3시간 전) ab1a091
* (3시간 전) b44a769
* (3시간 전) 93c7114
* (3시간 전) 54d0235
* (3시간 전) 77c0b5b
* (3시간 전) e5143b1
* (3시간 전) 4c1a5db
* (3시간 전) a288ffb
* (3시간 전) b86b554
* (3시간 전) 75942cd
* (3시간 전) dece25b
* (3시간 전) 3e293e3
* (3시간 전) e21c75b
* (3시간 전) c79629d
| * (6시간 전) 20ad4e2
| * (7시간 전) 88fa156
| * (8시간 전) 063c591
| * (8시간 전) c8c7df7
| * (8시간 전) 282f7b9
| * (8시간 전) fae9472
| * (8시간 전) 4a0981a
| * (8시간 전) b500bf4
| * (9시간 전) 3edc586
| * (9시간 전) e7027c5
| * (9시간 전) 643d23a
| * (9시간 전) 4725f0f
| * (9시간 전) e32ed20
| * (9시간 전) db03f27
| * (9시간 전) 5b84a5f
| * (9시간 전) 1a3949f
| * (10시간 전) 365b2e1
| * (11시간 전) 62b965b
| * (11시간 전) 215e993
| * (11시간 전) 9ba8cba
| * (11시간 전) 3235509
|/
* (12시간 전) 93e9e47
* 
```
```sh
$ git log branch/a..branch/b
* (3시간 전) c003c2c
* (3시간 전) c338742
* (3시간 전) ab1a091
* (3시간 전) b44a769
* (3시간 전) 93c7114
* (3시간 전) 54d0235
* (3시간 전) 77c0b5b
* (3시간 전) e5143b1
* (3시간 전) 4c1a5db
* (3시간 전) a288ffb
* (3시간 전) b86b554
* (3시간 전) 75942cd
* (3시간 전) dece25b
* (3시간 전) 3e293e3
* (3시간 전) e21c75b
* (3시간 전) c79629d
```
```sh
$ git rebase -i \
 $(git merge-base master branch/air) # vimwiki [[issue]] 로 2줄 작성
```

## --ancestry-path
> HEAD 가 아닌 중간으로 checkout 한 경우 HEAD(SHA_02) 까지 전진
```sh
$ git log --ancestry-path [SHA_01]..HEAD
```

```sh
$ git merge-base branch/a branch/b
93e9e47
```
```sh
$ git log --ancestry-path 93e9e47..branch/b
* (3시간 전) c003c2c
* (3시간 전) c338742
* (3시간 전) ab1a091
* (3시간 전) b44a769
* (3시간 전) 93c7114
* (3시간 전) 54d0235
* (3시간 전) 77c0b5b
* (3시간 전) e5143b1
* (3시간 전) 4c1a5db
* (3시간 전) a288ffb
* (3시간 전) b86b554
* (3시간 전) 75942cd
* (3시간 전) dece25b
* (3시간 전) 3e293e3
* (3시간 전) e21c75b
* (3시간 전) c79629d
```
```sh
$ git 
```

## untracked 파일 지우기
```sh
git clean -fd
```
- `-f` : 파일
- `-d` : 디렉토리

## url 변경하기
하드코딩된 url 때문에 다중 계정을 이용하는 경우 고통받을 수 있다.
다중 계정은 로컬에서key 파일을 통해서 인증 정보를 관리할 텐데 이런 경우 ssh 가 사용된다.

`[[.gitconfig-url]]` 을 설정한다.

## feature 브랜치 변경된 파일들 stage 화
### reset 후
```sh
git switch feature
git reset --mixed $(git merge-base master @)
```

#### 1. 변경된 파일 모두 버퍼로
`vim $(git diff --name-only)`
#### 2. 상태값으로 필터
`vim $(git status -s | grep "[^D] " | awk '{print $2}')`

### 보기만
`vim $(git diff --name-status master.. | rg "[^D]\t" | awk '{print $[[2]]}')`

## plumping 명령어
- `git rev-parse`: 브랜치명이나 태그를 커밋 해시로 리턴한다.
- `git rev-list`: git log 등의 레인지를 가지는 결과를 기반으로 커밋 해시만 뽑아낸다.
  - `--abbrev-commit` 옵션 가능

## releated
- [[github]]
- [[vim]]
- [[tig]]
- [[alias]]
- [[git-alias]]
