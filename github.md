# github

## ssh
특정 상황에서 `https` 사용이 강제되어 문제가 발생하는 경우 [[.gitconfig-url]] 을 설정한다.
[[env]]
```sh
GIT_SSH_COMMAND='ssh -i [KEY_PATH]' bundle exec fastlane build phase:dev
```

## link
- [[git]]
- [[gh]]
- [[webhook]]
