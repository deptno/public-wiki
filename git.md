# Git

분산저장관리
공식문서: https://git-scm.com/book/ko/v2

[[##]] 특정 커밋 내용 가져와서 적용
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

## 검색`
https://stackoverflow.com/questions/2928584/how-to-grep-search-committed-code-in-the-git-history

```sh
git grep [REGEXP]
```

## 커밋 메시지 검색
```sh
git log --grep=[REGEXP]`
```
```sh
$ git log --grep=aaaaaaaaa

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
커밋 SHA로 검색을 하게 되면 **정상적인 리버트 커밋이 있는 경우** 함께 검색된다.

## 코드 검색
```sh
git grep -pPn [REGEXP]
```

### options
- `-P` : perl compatible regular expression
- `-n` : 라인넘버를 포함하여 출력
- `-p` : 함수 이름을 포함
- `-F` : fixed string 형태로 검색한다. regexp 가 아닌겨우

## 깃에서 관리하는 파일인지 확인
```sh
git ls-files [FILE]
```

## 깃에서 무시하는 파일인지 확인
```sh
git check-ignore -v [FILE]
```

```sh
git status --ignored [FILE]
```
후자가 최신
