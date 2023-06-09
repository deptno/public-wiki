# frum

ruby package manager
rust 로 작성됨

[[path|~/.frum]] 디렉토리에 환경이 저장된다.

*2022-01-07*
*결과적으로 되지 않아서 chruby + ruby-install 조합으로 갈아탐*
- chruby@0.3.9
- frum@0.12.0

```sh
brew install frum
```

```sh
ruby -v
frum install --list
frum install 3.0.3
frum local 3.0.3
frum uninstall 3.0.3
ruby -v
```

## [[error|트러블슈팅]]
- https://github.com/TaKO8Ki/frum/issues/94#issuecomment-898195336

```sh
$ bundle install                                                           ok  20:28:16
Could not load OpenSSL.
You must recompile Ruby with OpenSSL support or change the sources in your Gemfile from 'https' to 'http'. Instructions for compiling with OpenSSL
using RVM are available at rvm.io/packages/openssl.
```
```sh
brew install openssl
frum uninstall [VERSION]
frum install [VERSION]
```

## link
- [[ruby]]
- [[rust]]
- [[chruby]]
