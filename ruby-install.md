# ruby-install

- https://github.com/postmodern/ruby-install

`~/.rubies` 에 여러 버전의 루비 설치가 가능하다.
[[chruby]] 에서 사용된다.

## 3.x
```sh
$ ruby-install                                                    INT  2m 42s  00:00:18
>>> Downloading latest ruby versions ...
>>> Downloading latest jruby versions ...
>>> Downloading latest rbx versions ...
>>> Downloading latest truffleruby versions ...
>>> Downloading latest truffleruby-graalvm versions ...
>>> Downloading latest mruby versions ...
Stable ruby versions:
  ruby:
    2.6.9
    2.7.5
    3.0.3
    3.1.0
  jruby:
    9.3.2.0
  rbx:
    5.0
  truffleruby:
    21.3.0
  truffleruby-graalvm:
    21.3.0
  mruby:
    3.0.0
$ arch -arm64 ruby-install 3.1.0

## 2.x
그냥 인스톨하는 경우 m1 에서 [[error|에러]] 를 내뿜는다.
```sh
$ ruby-install ruby 2.7.5

compiling readline.c
readline.c:1904:37: error: use of undeclared identifier 'username_completion_function'; did you mean 'rl_username_completion_function'?
                                    rl_username_completion_function);
                                    ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                                    rl_username_completion_function
readline.c:79:42: note: expanded from macro 'rl_username_completion_function'
# define rl_username_completion_function username_completion_function
                                         ^
/opt/homebrew/opt/readline/include/readline/readline.h:485:14: note: 'rl_username_completion_function' declared here
extern char *rl_username_completion_function PARAMS((const char *, int));
             ^
1 error generated.
make[2]: *** [readline.o] Error 1
make[1]: *** [ext/readline/all] Error 2
make: *** [build-ext] Error 2
!!! Compiling ruby 2.7.5 failed!
```

이슈 해결은 여길 참조 https://github.com/postmodern/ruby-install/issues/399
```sh
$ CFLAGS="-Wno-error=implicit-function-declaration" arch -arm64 ruby-install ruby 2.7.5
```

[[arch-arm64|arch -arm64]] 옵션은 [[m1]] 프로세서일때 필요하다.

## link
- [[ruby]]
- [[chruby]]
