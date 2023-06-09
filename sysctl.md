# sysctl

시스템 변수를 읽고 쓴다.

## 사용법
```sh
# read
sysctl VARIABLE_NAME
# write
sudo sysctl -w VARIABLE_NAME=value
```

## 사용
```sh
# 프로세서 [[이름]]
sysctl -n machdep.cpu.brand_string
sysctl -n machdep.cpu
# 열수있는 fd 관리, macOS 10.6 이전에서만 사용되었음
# + 참고. https://facebook.github.io/watchman/docs/install.html#mac-os-file-descriptor-limits
sudo sysctl -w kern.maxfiles=10485760
sudo sysctl -w kern.maxfilesperproc=1048576
```

## link
- [[macos]]
- [[watchman]]
