# git-commit

## options
- -a 추적중인 파일들을 스테이징하여 커밋에 포함시킴
- -v 변경사항을 표시
- -p `git add -p` 와 같은 것으로 보임 commit 시에 진진행
- -m message 를 터미널에서 남길때 사용한다
- --allow-empty - force 푸시 하기 어려울때 이력을 내용을 담아 commit 할때도 쓰임
  + https://github.com/deptno/public_wiki/commit/fec27017a7f78f8cac3508ebd181e93d6eb560bd 
- --no-verify 훅 무시

## 빈 커밋
-  feature 를 처리하다가 다른일을 하게 될 때
  - 잊어버리는 경우가 있어서 빈 커밋을 남겨두어 두면 도움이된다 i.g. wip
  - 커밋 내용에 현재 하던 내용을 글로 정리할 수 있다
```sh
git commit --allow-empty -m "wip: replace this"
```

## link
- [[git]]
- [[git-add]]
