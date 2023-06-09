# gem

```sh
# 버전 지정
gem install bundler:2.2.20
gem install bundler:2.2.20 --with-openssl-dir=$(brew --prefix openssl@1.1)
gem install bundler:2.2.20 --source=https://rubygems.org
gem info bundler
gem search bundler
gem update bundler
gem info bundler
gem list --local
gem uninstall bundler
# Gemfile 에 없는 애들은 모두 제거
gem cleanup
```

## [[env]]
- `GEM_PATH`
- `GEM_HOME`
- `GEM_ROOT`

## link
- [[ruby]]
- [[ruby-gems|ruby gems]]
- [[bundler]]
