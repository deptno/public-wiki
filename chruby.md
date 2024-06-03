# chruby

- https://github.com/postmodern/chruby

ruby package manager

## file
- [[path|.ruby-version]]
- [[path|.rubies]]

## 설치
```sh
$ brew install chruby

$ vim ~/.zshrc
source /opt/homebrew/opt/chruby/share/chruby/chruby.sh
source /opt/homebrew/opt/chruby/share/chruby/auto.sh

$ echo "ruby-2.7.5" > ~/.ruby-version
```

실행시 `auto.sh` 를 통해 `.ruby-version` 을 읽어서 버전을 세팅한다.
해당 버전의 루비 인스톨은 [[ruby-install]] 을 통해서 진행한다.

해당 디렉토리부터 상위 디렉토리로 올라가면서 찾는 구조,  
때문에 `$HOME` 에 하나 특정 프로젝트에 필요한대로 설정하면 된다.

## 루비 인스톨
```sh
ruby-install stable
ruby-install 3.1.0
arch-arm64 ruby-install 3.1.0
ruby-install ruby 2.7.5
```

## default ruby 설정
```sh 
# 설치된 버전중 하나로 설정해야한다
chruby ruby-3.3.2
```
### default 버전 설정
- auto-switching(`auto.sh`) 이 설정되어 있는 경우 home 에 를 만들어서 설정할 수 있다. `.ruby-version`
- zshrc 에서 `chruby ruby-3.3.2` 와 같이 원하는 버전을 설정한다

## link
- [[ruby-install]]
