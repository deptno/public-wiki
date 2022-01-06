# chruby

- https://github.com/postmodern/chruby

ruby package manager

```sh
$ brew install chruby

$ vim ~/.zshrc
source /opt/homebrew/opt/chruby/share/chruby/chruby.sh
source /opt/homebrew/opt/chruby/share/chruby/auto.sh

$ echo "ruby-2.7.5" > ~/.ruby-version
```

실행시 `auto.sh` 를 통해 `.ruby-version` 을 읽어서 버전을 세팅한다.
해당 버전의 루비 인스톨은 [[ruby-install]] 을 통해서 진행한다.

## related
- [[ruby-install]]
