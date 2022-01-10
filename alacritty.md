# alacritty

gpu 가속을 지원한다.
m1 은 0.9 기준으로 빌드는 가능하나 [[brew]] 를 통해서는 지원하지 않는다.
`~/.config/alacritty/alacritty.yml` 에서 설정이 가능하기 때문에 [[git]] 을 통환 관리에 이점이 있다.

## [[m1]] build
코드의 최신버전으로 빌드한다.
- https://github.com/alacritty/alacritty/pull/4727#issuecomment-937892278
- https://github.com/alacritty/alacritty/tags - 안정화 버전 빌드를 원하면 태그를 사용

```sh
git clone git@github.com:alacritty/alacritty.git && \
cd alacritty && \
rustup update && \
rustup target add x86_64-apple-darwin && \
rustup target add aarch64-apple-darwin && \ cargo check --target=x86_64-apple-darwin && \
cargo check --target=aarch64-apple-darwin && \
make dmg-universal && \
mv /Applications/Alacritty{@x86_64}.app 
cp -r target/release/osx/Alacritty.app /Applications/
mv /Applications/Alacritty{@arm64}.app 
```

```
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
빌드 후 결과를 비교해 보면
```sh
.rw-r--r--      1 4.3k deptno 2022-01-10 23:32    │  │  ├──  alacritty.fish
.rw-r--r--      1 5.7k deptno 2022-01-10 23:32    │  │  ├──  alacritty.bash
.rw-r--r--      1 4.7k deptno 2022-01-10 23:32    │  │  └──  _alacritty
```
이쪽에 퍼미션이 없는 것을 확인할 수 있다. 추가적으로 퍼미션을 부여한다.
```sh
chmod +x /Applications/Alacritty@arm64.app/Contents/Resources/completions/*
```

# relatead
- [[rust]]
- [[brew]]
- [[m1]]
- [[nvim-treesitter]]
