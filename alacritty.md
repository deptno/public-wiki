# alacritty

gpu 가속을 지원한다.
m1 은 0.9 기준으로 빌드는 가능하나 [[brew]] 를 통해서는 지원하지 않는다.
`~/.config/alacritty/alacritty.yml` 에서 설정이 가능하기 때문에 [[git]] 을 통환 관리에 이점이 있다.

## [[m1]] build
코드의 최신버전으로 빌드한다.
- https://github.com/alacritty/alacritty/pull/4727#issuecomment-937892278
- https://github.com/alacritty/alacritty/tags - 안정화 버전 빌드를 원하면 태그를 사용

```sh
git clone git@github.com:alacritty/alacritty.git
cd alacritty
git checkout v0.9.0
rustup update
rustup target add x86_64-apple-darwin aarch64-apple-darwin
make app-universal
cp -r target/release/osx/Alacritty.app /Applications/
cp extra/completions/_alacritty ${ZDOTDIR:-~}/.zsh_functions/_alacritty
```

post build 프로세스 있으므로 추가적으로 할 것이 있는지 확인한다.
macos 기준으로는 별 의미를 찾지 못하겠다.
- https://github.com/alacritty/alacritty/blob/master/INSTALL.md

```sh
Permissions Links Size User   Date Modified    Name
drwxr-xr-x      3    - deptno 2022-01-10 23:35  Alacritty@arm64.app
drwxr-xr-x      5    - deptno 2022-01-10 23:32 └──  Contents
drwxr-xr-x      3    - deptno 2022-01-10 23:35    ├──  MacOS
.rwxr-xr-x      1 8.4M deptno 2022-01-10 23:35    │  └──  alacritty
drwxr-xr-x      7    - deptno 2022-01-10 23:32    ├──  Resources
drwxr-xr-x      5    - deptno 2022-01-10 23:35    │  ├──  completions
.rw-r--r--      1 4.3k deptno 2022-01-10 23:32    │  │  ├──  alacritty.fish
.rw-r--r--      1 5.7k deptno 2022-01-10 23:32    │  │  ├──  alacritty.bash
.rw-r--r--      1 4.7k deptno 2022-01-10 23:32    │  │  └──  _alacritty
drwxr-xr-x      4    - deptno 2022-01-10 23:35    │  ├──  61
.rw-r--r--      1 3.5k deptno 2022-01-10 23:35    │  │  ├──  alacritty-direct
.rw-r--r--      1 3.5k deptno 2022-01-10 23:35    │  │  └──  alacritty
.rw-r--r--      1  670 deptno 2022-01-10 23:35    │  ├──  alacritty-msg.1.gz
.rw-r--r--      1 1.2k deptno 2022-01-10 23:35    │  ├──  alacritty.1.gz
.rw-r--r--      1 666k deptno 2022-01-10 23:32    │  └──  alacritty.icns
.rw-r--r--      1 2.5k deptno 2022-01-10 23:32    └──  Info.plist
drwxr-xr-x@     3    - deptno 2021-08-03 18:52  Alacritty@x86_64.app
drwxr-xr-x@     5    - deptno 2021-08-03 18:47 └──  Contents
drwxr-xr-x@     3    - deptno 2021-08-03 18:52    ├──  MacOS
.rwxr-xr-x@     1 4.1M deptno 2021-08-03 18:52    │  └──  alacritty
drwxr-xr-x@     6    - deptno 2021-08-03 18:47    ├──  Resources
drwxr-xr-x@     5    - deptno 2021-08-03 18:52    │  ├──  completions
.rwxr-xr-x@     1 1.6k deptno 2021-08-03 18:47    │  │  ├──  alacritty.bash
.rwxr-xr-x@     1 1.0k deptno 2021-08-03 18:47    │  │  ├──  _alacritty
.rwxr-xr-x@     1 1.6k deptno 2021-08-03 18:47    │  │  └──  alacritty.fish
drwxr-xr-x@     4    - deptno 2021-08-03 18:52    │  ├──  61
.rw-r--r--@     1 3.5k deptno 2021-08-03 18:52    │  │  ├──  alacritty-direct
.rw-r--r--@     1 3.5k deptno 2021-08-03 18:52    │  │  └──  alacritty
.rw-r--r--@     1 1.2k deptno 2021-08-03 18:52    │  ├──  alacritty.1.gz
.rw-r--r--@     1 666k deptno 2021-08-03 18:47    │  └──  alacritty.icns
.rw-r--r--@     1 2.5k deptno 2021-08-03 18:47    └──  Info.plist
```

## error
~/Document ~/Download ~/Desktop 등의 디렉토리에서 permission 에러가 나는 경우

> [[system-preferences|시스템 환경설정]] -> 보안 및 개인 정보 보호 -> 파일 및 폴더
에서 명령어 개별로 되어있는 경우가 있다면 제거한다. eg. zsh, exa, ls 등

[[codesign]] 처리한다.

# relatead
- [[rust]]
- [[brew]]
- [[m1]]
- [[nvim-treesitter]]
- [[infocmp]]
- [[codesign]]
