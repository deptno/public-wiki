# react-native

XXApp 으로 이름 가정

```sh
$ react-native run ios
info Found Xcode workspace "XXApp.xcworkspace"
xcrun: error: unable to find utility "simctl", not a developer tool or in PATH
error Could not get the simulator list from Xcode. Please open Xcode and try running project directly from there to resolve the remaining issues.
Error: Command failed: xcrun simctl list --json devices
xcrun: error: unable to find utility "simctl", not a developer tool or in PATH

    at checkExecSyncError (node:child_process:826:11)
    at Object.execFileSync (node:child_process:864:15)
    at runOnSimulator (/[APP_PATH]/node_modules/@react-native-community/cli-platform-ios/build/commands/runIOS/index.js:167:54)
    at Object.runIOS [as func] (/[APP_PATH]/node_modules/@react-native-community/cli-platform-ios/build/commands/runIS/index.js:121:12)
    at Command.handleAction (/[APP_PATH]/node_modules/react-native/node_modules/@react-native-community/cli/build/index.js:192:23)
    at Command.listener (/[APP_PATH]/node_modules/commander/index.js:315:8)
    at Command.emit (node:events:390:28)
    at Command.parseArgs (/[APP_PATH]/node_modules/commander/index.js:651:12)
    at Command.parse (/[APP_PATH]/node_modules/commander/index.js:474:21)
    at setupAndRun (/[APP_PATH]/node_modules/react-native/node_modules/@react-native-community/cli/build/index.js:271:24)
info Run CLI with --verbose flag for more details.
```

```sh
xcode-select -p
/Library/Developer/CommandLineTools
# 결과가 위와 같은 경우 아래 커맨드를 입력
sudo xcode-select --switch /Applications/Xcode.app
```

```sh
error Failed to build iOS project. We ran "xcodebuild" command but it exited with error code 65. To debug build logs further, consider building your app with Xcode.app, by opening XXApp.xcworkspace.
```

xcode 에서 팀 설정 필요 [[error]]




## related
- [[mobile]]
- [[xcode]]
