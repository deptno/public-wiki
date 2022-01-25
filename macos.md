# macos

## app
- [[karabiner-elements]]

## [[terminal|터미널]] 명령어 
- [[pmset]]
- defaults-read
- [[sysctl]]
- [[uname]]
- [[powermetrics]]


```sh
# 프로세서 이름
sysctl -n machdep.cpu.brand_string
# 현재 사용중인 키보드 레이아웃 상태
defaults read ~/Library/Preferences/com.apple.HIToolbox.plist AppleSelectedInputSources
# 센서 정보들
pmset -g therm
```

```sh
defaults read ~/Library/Preferences/com.apple.HIToolbox.plist AppleSelectedInputSources | grep -i "keyboardlayout name" | sed 's/KeyboardLayout Name = (\s);/_/'
```

## [[error]]
- [[codesign|퍼미션을 계속 물어보는 경우]]

## versions
- [[macos-monterey|몬터레이]]

## setup
- [[setup-terminal|맥 터미널 설정]]
