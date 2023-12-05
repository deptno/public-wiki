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

## link
- [[git]]
- [[gh]]
- [[webhook]]
