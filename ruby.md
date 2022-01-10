# ruby

## system ruby
mac 을 설치하면 기본적으로 루비가 설치되어있다. 

## support archtecture
```sh
$ ruby -v
ruby 2.6.8p205 (2021-07-07 revision 67951) [universal.x86_64-darwin21]
```
[[universal]] 을 참고한다.

```sh
$ echo "ruby-3.1.0" > .ruby-version
$ ruby -v
ruby 3.1.0p0 (2021-12-25 revision fb4df44d16) [arm64-darwin21]
 ```
[[chruby]] 를 사용하여 버전을 변경해보았따. 여기에는 [[arm64]] 가 찍혀있다.

native 빌드가 당연히 성능적인 이점이 있겠지만,  
관련된 [[gem]] 들이 모든 호환성이 보장되는지는 알 수 없다.

## [[ruby-gems|ruby]] gems vs [[bundler]] *정확하지 않음*
루비는 [[ruby-gems]] 를 가지고 있지만 패키지 파일([[path|Gemfile]], [[path|Gemfile.lock]])을 지원하지 않는다.

때문에 정확히 동일한 버저닝을 통해 여러 디펜던시를 관리하기 위해서는 이를 관리할 매니저가 필요한데 [[bundler]] 가 이를 행한다.
[[bundler]] 또한 하나의 [[gem]] 으로 버전을 가진다.

일단 [[bundler]] 가 설치되면 이후부터는 `bundle install` 을 통해 [[gem]] 들의 로컬(프로젝트 스코프) 설치가 가능해진다.
각 [[gem]] 들은 `bundle exe GEM` 와 같은 형태로 사용이 가능하다.

## [[env]]
- `GEM_PATH`
- `GEM_HOME`

## [[error|트러블슈팅]]
system ruby 를 사용해서 설치를 하는 경우 bundler 가 존재하지 않고 이를 설치하려고하면 결국 시스템 퍼미션 권한을 요구한다.
[[frum]] 같은 ruby version manager 를 사용해서 따로 버전을 설치하고 로컬로 관리하는 것이 좋을 것으로 생각된다.

```sh
$ bundle exec pod install
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems.rb:283:in `find_spec_for_exe': Could not find 'bundler' (2.2.20) required by your /APP/ios/Gemfile.lock. (Gem::GemNotFoundException)
To update to the latest version installed on your system, run `bundle update --bundler`.
To install the missing version, run `gem install bundler:2.2.20`
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems.rb:302:in `activate_bin_path'
        from /usr/bin/bundle:23:in `<main>`'
        
$ bundle install
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems.rb:283:in `find_spec_for_exe': Could not find 'bundler' (2.2.20) required by your /APP/ios/Gemfile.lock. (Gem::GemNotFoundException)
To update to the latest version installed on your system, run `bundle update --bundler`.
To install the missing version, run `gem install bundler:2.2.20`
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems.rb:302:in `activate_bin_path'
        from /usr/bin/bundle:23:in `<main>`'
        
$ gem install bundler:2.2.20
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

## related
- [[bundler]]
- [[ruby-gems]]
- [[frum]]
- [[chruby]]
- [[universal]]
- [[x86_64]]
- [[arm64]]
