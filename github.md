# github

## ssh
특정 상황에서 `https` 사용이 강제되어 문제가 발생하는 경우 [[.gitconfig-url]] 을 설정한다.
[[env]]
```sh
GIT_SSH_COMMAND='ssh -i [KEY_PATH]' bundle exec fastlane build phase:dev
```

## project
> 굉장히 괜찮다

- pros
  - repository 독립적
  - table 지원
  - kanban 지원
  - gantt 지원
  - filter 지원

## github pages
- PAT 토큰은 클래식으로 생성해야하고 repo 권한을 줘야지만 동작한다
  + https://github.com/deptno/public_wiki/commit/1751619a2d37c0440affc3c74bb316554dc4e4c6
  + https://github.com/settings/tokens
- 계정 설정에서 토큰 생성후 레포설정에서 환경 주입

## link
- [[git]]
- [[gh]]
- [[webhook]]
