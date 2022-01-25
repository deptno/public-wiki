# bundler

bundler 는 [[path|Gemfile]] 일에 명시된 gem 을 설치한다.
설치되는 위치는 bundle config 를 우선한다
default config 는 [[ruby-gems]] 의 환경이 사용된다. $[[env|GEM_HOME]] 

https://bundler.io/v2.2/man/bundle-config.1.html

[[@todo]] 비교 필요

```sh
gem install bundler
bundle install
bundle exe pod install
```

```sh
gem install bundler
bundle config set --local path 'vendor/bundle'
bundle install
bundle exe pod install
```

## related
- [[ruby]]
- [[ruby-gems|ruby gems]]
- [[react-native]]
- [[cocoapods]]
