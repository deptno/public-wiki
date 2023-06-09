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
### ffi
```sh
 ~/.gem/ruby/2.6.8/gems/ffi-1.13.1/lib  ls                                                                                      ok  17:54:54
ffi          ffi.rb       ffi_c.bundle
 ~/.gem/ruby/2.6.8/gems/ffi-1.13.1/lib  file ./ffi                                                                              ok  17:54:56
./ffi: directory
 ~/.gem/ruby/2.6.8/gems/ffi-1.13.1/lib  file ffi_c.bundle                                                                       ok  17:55:03
ffi_c.bundle: Mach-O 64-bit bundle x86_64
```
```sh
gem dependency -R
Gem ffi-1.13.1
  rake (~> 13.0, development)
  rake-compiler (~> 1.0, development)
  rake-compiler-dock (~> 1.0, development)
  rspec (~> 2.14.1, development)
  rubygems-tasks (~> 0.2.4, development)
  Used by
    ethon-0.12.0 (ffi (>= 1.3.0))
```
### mac13 ventura
```sh
Installing json 2.6.2 with native extensions
Installing unf_ext 0.0.8.2 with native extensions
Installing ffi 1.15.5 with native extensions
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

    current directory: /Users/deptno/.gem/ruby/2.6.8/gems/json-2.6.2/ext/json/ext/generator
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/bin/ruby -I /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0 -r ./siteconf20221031-78847-d4qa86.rb extconf.rb
creating Makefile

current directory: /Users/deptno/.gem/ruby/2.6.8/gems/json-2.6.2/ext/json/ext/generator
make "DESTDIR=" clean

current directory: /Users/deptno/.gem/ruby/2.6.8/gems/json-2.6.2/ext/json/ext/generator
make "DESTDIR="
compiling generator.c
In file included from generator.c:1:
In file included from ./../fbuffer/fbuffer.h:5:
In file included from /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX12.3.sdk/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/include/ruby-2.6.0/ruby.h:33:
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX12.3.sdk/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/include/ruby-2.6.0/ruby/ruby.h:24:10: fatal error: 'ruby/config.h'
file not found
#include "ruby/config.h"
         ^~~~~~~~~~~~~~~
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX12.3.sdk/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/include/ruby-2.6.0/ruby/ruby.h:24:10: note: did not find header
'config.h' in framework 'ruby' (loaded from '/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks')
1 error generated.
make: *** [generator.o] Error 1

make failed, exit code 2

Gem files will remain installed in /Users/deptno/.gem/ruby/2.6.8/gems/json-2.6.2 for inspection.
Results logged to /Users/deptno/.gem/ruby/2.6.8/extensions/universal-darwin-22/2.6.0/json-2.6.2/gem_make.out

  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:99:in `run'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:51:in `block in make'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:43:in `each'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:43:in `make'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/ext_conf_builder.rb:62:in `block in build'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/tempfile.rb:295:in `open'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/ext_conf_builder.rb:29:in `build'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:185:in `block in build_extension'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/monitor.rb:235:in `mon_synchronize'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:181:in `build_extension'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:229:in `block in build_extensions'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:226:in `each'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:226:in `build_extensions'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/installer.rb:830:in `build_extensions'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/rubygems_gem_installer.rb:71:in `build_extensions'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/rubygems_gem_installer.rb:28:in `install'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/source/rubygems.rb:204:in `install'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/installer/gem_installer.rb:54:in `install'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/installer/gem_installer.rb:16:in `install_from_spec'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/installer/parallel_installer.rb:186:in `do_install'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/installer/parallel_installer.rb:177:in `block in worker_pool'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:62:in `apply_func'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:57:in `block in process_queue'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:54:in `loop'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:54:in `process_queue'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:91:in `block (2 levels) in create_threads'

An error occurred while installing json (2.6.2), and Bundler cannot continue.

In Gemfile:
  cocoapods was resolved to 1.11.2, which depends on
    cocoapods-core was resolved to 1.11.2, which depends on
      algoliasearch was resolved to 1.27.5, which depends on
        json


Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

    current directory: /Users/deptno/.gem/ruby/2.6.8/gems/unf_ext-0.0.8.2/ext/unf_ext
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/bin/ruby -I /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0 -r ./siteconf20221031-78847-fkkg4h.rb extconf.rb
checking for -lstdc++... *** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
        --with-opt-dir
        --without-opt-dir
        --with-opt-include
        --without-opt-include=${opt-dir}/include
        --with-opt-lib
        --without-opt-lib=${opt-dir}/lib
        --with-make-prog
        --without-make-prog
        --srcdir=.
        --curdir
        --ruby=/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/bin/$(RUBY_BASE_NAME)
        --with-static-libstdc++
        --without-static-libstdc++
        --with-stdc++lib
        --without-stdc++lib
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:467:in `try_do': The compiler failed to generate an executable file. (RuntimeError)
You have to install development tools first.
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:546:in `block in try_link0'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/tmpdir.rb:93:in `mktmpdir'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:543:in `try_link0'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:570:in `try_link'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:789:in `try_func'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:1016:in `block in have_library'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:959:in `block in checking_for'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:361:in `block (2 levels) in postpone'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:331:in `open'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:361:in `block in postpone'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:331:in `open'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:357:in `postpone'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:958:in `checking_for'
        from /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/mkmf.rb:1011:in `have_library'
        from extconf.rb:6:in `<main>'

To see why this extension failed to compile, please check the mkmf.log which can be found here:

  /Users/deptno/.gem/ruby/2.6.8/extensions/universal-darwin-22/2.6.0/unf_ext-0.0.8.2/mkmf.log

extconf failed, exit code 1

Gem files will remain installed in /Users/deptno/.gem/ruby/2.6.8/gems/unf_ext-0.0.8.2 for inspection.
Results logged to /Users/deptno/.gem/ruby/2.6.8/extensions/universal-darwin-22/2.6.0/unf_ext-0.0.8.2/gem_make.out

  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:99:in `run'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/ext_conf_builder.rb:47:in `block in build'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/tempfile.rb:295:in `open'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/ext_conf_builder.rb:29:in `build'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:185:in `block in build_extension'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/monitor.rb:235:in `mon_synchronize'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:181:in `build_extension'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:229:in `block in build_extensions'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:226:in `each'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/ext/builder.rb:226:in `build_extensions'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/installer.rb:830:in `build_extensions'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/rubygems_gem_installer.rb:71:in `build_extensions'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/rubygems_gem_installer.rb:28:in `install'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/source/rubygems.rb:204:in `install'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/installer/gem_installer.rb:54:in `install'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/installer/gem_installer.rb:16:in `install_from_spec'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/installer/parallel_installer.rb:186:in `do_install'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/installer/parallel_installer.rb:177:in `block in worker_pool'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:62:in `apply_func'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:57:in `block in process_queue'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:54:in `loop'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:54:in `process_queue'
  /Users/deptno/.gem/ruby/2.6.8/gems/bundler-2.3.5/lib/bundler/worker.rb:91:in `block (2 levels) in create_threads'

An error occurred while installing unf_ext (0.0.8.2), and Bundler cannot continue.

In Gemfile:
  fastlane was resolved to 2.210.1, which depends on
    faraday-cookie_jar was resolved to 0.0.7, which depends on
      http-cookie was resolved to 1.0.5, which depends on
        domain_name was resolved to 0.5.20190701, which depends on
          unf was resolved to 0.1.4, which depends on
            unf_ext
```
-> [[chruby]] 를 사용해서 [[ruby]] 2.7.5 버전으로 업데이트
-> [[bundler]] 2.3.8 버전 업데이트
-> 해결

## link
- [[bundler]]
- [[ruby-gems]]
- [[frum]]
- [[chruby]]
- [[universal]]
- [[x86_64]]
- [[arm64]]
