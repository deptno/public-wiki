# gem

```sh
# 버전 지정
gem install bundler:2.2.20
gem install bundler:2.2.20 --with-openssl-dir=$(brew --prefix openssl@1.1)
gem install bundler:2.2.20 --source=https://rubygems.org
# Gemfile 에 없는 애들은 모두 제거
gem cleanup
```

## [[env]]
- `GEM_PATH`
- `GEM_HOME`

## related
- [[ruby]]
- [[ruby-gems|ruby gems]]
- [[bundler]]
