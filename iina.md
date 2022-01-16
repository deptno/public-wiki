# iina

[[mpv]] frontend

```sh
brew install mpv-iina
```

## [[error]]
```sh
==> /opt/homebrew/opt/python@3.9/bin/python3 waf configure --prefix=/opt/homebrew/Cellar/mpv-iina/0.34.0 --enable-javascript --enable-libmpv-s
Last 15 lines from /Users/deptno/Library/Logs/Homebrew/mpv-iina/02.python3:
  File "/private/tmp/mpv-iina-20220111-51684-45t55q/mpv-0.34.0/.waf3-2.0.20-36f5354d605298f6a89c09e0c7ef6c1d/waflib/Configure.py", line 85, in execute
    super(ConfigurationContext,self).execute()
  File "/private/tmp/mpv-iina-20220111-51684-45t55q/mpv-0.34.0/.waf3-2.0.20-36f5354d605298f6a89c09e0c7ef6c1d/waflib/Context.py", line 92, in execute
    self.recurse([os.path.dirname(g_module.root_path)])
  File "/private/tmp/mpv-iina-20220111-51684-45t55q/mpv-0.34.0/.waf3-2.0.20-36f5354d605298f6a89c09e0c7ef6c1d/waflib/Context.py", line 133, in recurse
    user_function(self)
  File "/private/tmp/mpv-iina-20220111-51684-45t55q/mpv-0.34.0/wscript", line 955, in configure
    ctx.load('detections.compiler_swift')
  File "/private/tmp/mpv-iina-20220111-51684-45t55q/mpv-0.34.0/.waf3-2.0.20-36f5354d605298f6a89c09e0c7ef6c1d/waflib/Configure.py", line 156, in load
    func(self)
  File "/private/tmp/mpv-iina-20220111-51684-45t55q/mpv-0.34.0/waftools/detections/compiler_swift.py", line 201, in configure
    __find_macos_sdk(ctx)
  File "/private/tmp/mpv-iina-20220111-51684-45t55q/mpv-0.34.0/waftools/detections/compiler_swift.py", line 158, in __find_macos_sdk
    minor = string.ascii_lowercase.index(version_parts.group(2).lower())
ValueError: substring not found

If reporting this issue please do so at (not Homebrew/brew or Homebrew/core):
  https://github.com/iina/homebrew-mpv-iina/issues
```
```sh
brew uninstall mpv
brew install mpv-inna
```

[[@todo]] 설치가 되나 그후로 실행하는 방법을 모르겟음
`brew install iina` 로 설치하면 터미널에서 [[mpv]] 가없는 것으로 보임

## related
- [[mac]]
- [[mpv]]
