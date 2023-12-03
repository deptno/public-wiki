# react-native

## 설정
> 글 작성 시점 `0.72.7`
- `--skip-install` 은 패키지 매니저를 갈아 끼우려는 목적
- `--npm` 은 패키지 매니저를 갈 아 끼우려는 목적
  - 목적과는 별개로 옵션을 주지 않으면 실행되지 않음 [[### Please make sure the template is valid]]
- `node_modules` 가 필요하기때문에 `yarn` `pnp` 를 사용한다면 주의

```sh 
npx react-native@latest init MyApp --directory my-app --skip-install --npm 
# or
bun x react-native@latest init MyApp --directory my-app --skip-install --npm 
```

## 실행
```sh
react-native run ios --device "phonename"
```
## 설정
### neovim
- [[dap]]
- rn 파일 컨벤션 지원
  + https://github.com/deptno/nvim/commit/dda74cfcada336b0eb80cb84e84baf8e441dacf5


## error
> XXApp 으로 이름 가정

### The operation couldn’t be completed. Unable to locate a Java Runtime.
```sh
error Failed to install the app. Make sure you have the Android development environment set up: https://reactnative.dev/docs/environment-setup.
Error: Command failed: ./gradlew app:installDefaultDebug -PreactNativeDevServerPort=8081
The operation couldn’t be completed. Unable to locate a Java Runtime.
Please visit http://www.java.com for information on installing Java.
```
-> 자바 설치
```sh
brew install --cask adoptopenjdk/openjdk/adoptopenjdk8
```
- https://reactnative.dev/docs/environment-setup

### xcrun: error: unable to find utility "simctl", not a developer tool or in PATH
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

### Error: EMFILE: too many open files, watch
```sh
 yarn dev                                                                           1 err  38s  11:33:15
● Validation Warning:

  Unknown option "server.enableVisualizer" with value true was found.
  This is probably a typing mistake. Fixing it will remove this message.


               ######                ######
             ###     ####        ####     ###
            ##          ###    ###          ##
            ##             ####             ##
            ##             ####             ##
            ##           ##    ##           ##
            ##         ###      ###         ##
             ##  ########################  ##
          ######    ###            ###    ######
      ###     ##    ##              ##    ##     ###
   ###         ## ###      ####      ### ##         ###
  ##           ####      ########      ####           ##
 ##             ###     ##########     ###             ##
  ##           ####      ########      ####           ##
   ###         ## ###      ####      ### ##         ###
      ###     ##    ##              ##    ##     ###
          ######    ###            ###    ######
             ##  ########################  ##
            ##         ###      ###         ##
            ##           ##    ##           ##
            ##             ####             ##
            ##             ####             ##
            ##          ###    ###          ##
             ###     ####        ####     ###
               ######                ######

                 Welcome to React Native!
                Learn once, write anywhere


node:events:368
      throw er; // Unhandled 'error' event
      ^

Error: EMFILE: too many open files, watch
    at FSEvent.FSWatcher._handle.onchange (node:internal/fs/watchers:204:21)
Emitted 'error' event on NodeWatcher instance at:
    at NodeWatcher.checkedEmitError (/APP/node_modules/sane/src/node_watcher.js:143:12)
    at FSWatcher.emit (node:events:390:28)
    at FSEvent.FSWatcher._handle.onchange (node:internal/fs/watchers:210:12) {
  errno: -24,
  syscall: 'watch',
  code: 'EMFILE',
  filename: null
}
```
watchman 을 설치하도록한다.

에러는 사라지지만 근본적인 원인인지는 불 분명하다. 어제는 vim에서도 같은 에러가 발생했다.  
근본적으로는 OS 설정일 것으로 보는데 watchman 이 이걸 어떻게 처리해야하는지는 알아봐야할 것 같다. [[@todo]]

sysctl 을 통해서 변수를 확인할 수 있다.

### warning: Building targets in manual order is deprecated - choose Dependency Order in scheme settings instead, or set DISABLE_MANUAL_TARGET_ORDER_BUILD_WARNING in any of the targets in the current scheme to suppress this warning
schema edit -> build -> dependency order

### Please make sure the template is valid
```sh 
✔ Downloading template
✖ Copying template
error Couldn't find the "/var/folders/yr/lb2jlrrd1fs7h6hn30n_ksvr0000gn/T/rncli-init-template-FoAnnF/node_modules/react-native/template.config.js file inside "react-native" template. Please make sure the template is valid. Read more: https://github.com/react-native-community/cli/blob/master/docs/init.md#creating-custom-template.
info Run CLI with --verbose flag for more details.
```
- `0.72.7` 에서 발생, `--npm` 옵션을 추가해서 해결

## link
- [[mobile]]
- [[xcode]]
- [[android-studio]]
- [[ruby]]
- [[watchman]]
- [[ios-deploy]]
- [[flipper]]
- [[react-native-debugger]]
- [[tauri]]
- [[flutter]]
- [[android]]
- [[ios]]
