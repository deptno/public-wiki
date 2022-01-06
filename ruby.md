# ruby

## [[ruby-gems|ruby]] gems vs [[bundler]] *정확하지 않음*
루비는 패키지매니저를 ruby gems 를 가지고 있다.
[[ruby-gems|ruby-gems]] 는 시스템 전반에 걸쳐서 사용되는 것으로 생각되며 이 때문에 프로젝트 별로 디펜던시 독립이 보장되지 않는다.
이를 위해 [[bundler]] 가 사용되는 것으로 보이며 아이러니하게도 이 또한 하나의 [[gem]] 으로 버전을 가진다.

일단 [[bundler]] 가 설치되면 이후부터는 `bundle install` 을 통해 [[gem]] 들의 로컬(프로젝트 스코프) 설치가 가능해진다.
각 [[gem]] 들은 `bundle exe GEM` 와 같은 형태로 사용이 가능하다.


## [[error|트러블슈팅]]

system ruby 를 사용해서 설치를 하는 경우 bundler 가 존재하지 않고 이를 설치하려고하면 결국 시스템 퍼미션 권한을 요구한다.
[[frum]] 같은 ruby version manager 를 사용해서 따로 버전을 설치하고 로컬로 관리하는 것이 좋을 것으로 생각된다.

```sh
$ bundle exec pod install
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems.rb:283:in `find_spec_for_exe': Could not find 'bundler' (2.2.20) required by your /Users/deptno/workspace/src/github.com/zigbang/zigbang-client/packages/zigbang-app/ios/Gemfile.lock. (Gem::GemNotFoundException)
To update to the latest version installed on your system, run `bundle update --bundler`.
To install the missing version, run `gem install bundler:2.2.20`
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems.rb:302:in `activate_bin_path'
        from /usr/bin/bundle:23:in `<main>`'
        
$ bundle install
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems.rb:283:in `find_spec_for_exe': Could not find 'bundler' (2.2.20) required by your /Users/deptno/workspace/src/github.com/zigbang/zigbang-client/packages/zigbang-app/ios/Gemfile.lock. (Gem::GemNotFoundException)
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
