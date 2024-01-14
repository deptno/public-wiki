# xcode

## 애플 개발자 등록
- [[apple#개발자 등록]] 참조

## code signing
> 개발자 등록 후진행한다,  개발자 등록이 안되면 [[testflight]] 를 포함한 배포단계가 동작하지 않는다

- 프로비저닝 프로파일은 다음을 포함있다
  - 개발자의 인증서(애플을 통해 인증되었기 때문에 폰에서 신뢰하기를 누르는 절차가 생략된다)
  - 앱의 bundle id
  - device uuid
- 기본적으로 개발자 인증서가 하나 발급되어있다. https://developer.apple.com 에서는 확인하지 못했으나 *xcworkspace* 를 열어서보면 확인된다
  - *Signing & Capabiliteis* -> *Signing* -> *Team* 에 보면 추가되어있는 것을 확인할 수 있다
  - 선택을 하게되면 *identifiers* 리스트에 추가된 것을 확인할 수 있다
    + https://developer.apple.com/account/resources/identifiers/list
  - Product -> Archive 해본다
    - [X] 이제 Validate 진행된다
    - [X] Release Testing
      - `xcarchive` 파일이 `/Users/[USER_NAME]/Library/Developer/Xcode/Archives/[DATE]` 쪽에 저장
      - 지정한 경로로 `ipa` 파일 생성
    - [ ] Testflight Internal Only
    - [ ] Testflight & Appstore
  - [ ] Product -> Edit Schema -> Release -> Run 하게도면 디바이스 미등록 메시지가 나오고 등록을 눌러준다
    - https://developer.apple.com/account/resources/devices/list 에 추가되는 것 확인
    - 설치 실패
    - `Failed to install the app on the device.`
      - `Command Ld emitted errors but did not return a nonzero exit code to indicate failure`
      - `IDERunOperationFailingWorker = IDEInstallCoreDeviceWorker;`
      - Team 선택에서 앱스토어가 아닌 개발자 등록 이전에 쓰던 것으로는 사용가능
      - 아마도 **인증서가 바뀐경우 설치된 디바이스에서 수동으로 제거** 가 필요한 것으로 추측
        + https://developer.apple.com/forums/thread/739711 pod 지우고 다시 설치하라는 글도 있다
    - [[testflight]] 설정
    - 일반 > VPN 및 기기 관리 -> VPN 쪽으로 이동한 것으로 추측됨

## 빌드
### 원격 빌드
1. xcode -> Window -> Devices and Simulators [shift + cmd + 2]
2. iPhone 을 케이블로 연결
3. Device ~~화면 상단의~~ `Connect via netowrk` 를 선택
  - 상단이 아니라 디바이스 우클릭으로 바뀜

## [[m1]] issue
Application 의 XCode 정보보기에서 rosetta2 를 통해서 실행시킬수 있다.(Intel)

react-native 프로젝트를 진행하는데 있어서 시도한 조합을 기록한다.

### 성공한 조합
- alacritty(0.9 intel) system ruby(2.6.8p universal)
- get install bundler(2.2.0 GEM_HOME 변경후 gem 을 통해 설치)
- bundle install - 어차피 로컬 프로그램 설치일 뿐이지만, 역시나 [[arch-arm64|arch -arm64]] 옵션은 아무런 영향을 못미친다.
- bundle exe pod install - 어차피 빌드를 로컬에서 하는 개념이지만 역시나 [[arch-arm64|arch -arm64]] 옵션은 아무런 영향을 못미친다.
- xcode(intel)

+
- xcode(arm64) 로도 동일하게 성공했다. alacritty(0.9 arm64)

intel 은 해당 빌드가 x86_64, rosetta2 를 통한 실행을 의미한다.

`arch -arm64` 를 사용하지 않는다. 종속성에 이슈가 있는 것인지 native xcode 에서 빌드에 실패했다.
```sh
Undefined symbol: _pb_ostream_from_buffer
Undefined symbol: _pb_encode
Undefined symbol: _OBJC_METACLASS_$_GPBMessage
Undefined symbol: _pb_encode_string
...
```

## 확인사항

### xcode-select
```sh
$ xcode-select -p
# 응답값이 `/Applications/Xcode.app` 이 아닌 경우 아래와 같이 설정한다.
$ sudo xcode-select --switch /Applications/Xcode.app
```

### 팀설정
```sh
... .xcconfig:1:1: unable to open file (in target "XXApp" in project "XXApp")
```
XXApp -> TARGETS -> XXApp -> Signing & Capabilities -> Team: `None`

## warning
```text
Building targets in manual order is deprecated - choose Dependency Order in scheme settings instead, or set DISABLE_MANUAL_TARGET_ORDER_BUILD_WARNING in any of the targets in the current scheme to suppress this warning
```
-> Edit Schema -> Build Order -> Dependency Order

## [[error]]
```text
/path/to/react-native-app/ios/Pods/Target Support Files/Pods-App/Pods-App.debug.xcconfig:1:1: unable to open file (in target "App" in project "App")
```
```sh
bundle exe pod deintegrate # 문제가 있었을시
bundle exe pod install
```

`bundle exe pod install` 을하고 나면 xcode 에서 Pod 의 색이 정상화되고 이 후 빌드시에는 해당 에러 사라짐

```text
error: The sandbox is not in sync with the Podfile.lock. Run 'pod install' or update your CocoaPods installation.
```

```text
Showing All Messages
: The Legacy Build System will be removed in a future release. You can configure the selected build system and this deprecation message in File > Workspace Settings.
```
File -> Workspace settings... -> **Build System: New Build System (Default)** 로 변경

### framework 업그레이드에 따른 에러
```sh
info Found Xcode workspace "ZigbangApp.xcworkspace"
2022-06-07 16:19:51.718 simctl[24788:5981206] CoreSimulator detected version change.  Framework version (802.6.1) does not match existing job version (802.6).  Attempting to remove the stale service in order to add the expected version.
error Could not get the simulator list from Xcode. Please open Xcode and try running project directly from there to resolve the remaining issues.
SyntaxError: Unexpected token I in JSON at position 0
    at JSON.parse (<anonymous>)
info Run CLI with --verbose flag for more details.
```
```sh
rm -rf ~/Library/Developer/Xcode/DerivedData/*
```

### ld: symbol(s) not found for architecture x86_64
시뮬레이터에서만 발생하고 실기기에서는 발생하지 않는다
```sh
❌  ld: symbol(s) not found for architecture x86_64
❌  clang: error: linker command failed with exit code 1 (use -v to see invocation)
error Failed to build iOS project. We ran "xcodebuild" command but it exited with error code 65. To debug build logs further, consider building your app with Xcode.app, by opening ZigbangApp.xcworkspace.
info Run CLI with --verbose flag for more details.
```

terminal 이 arm 으로 실행된 경우
- simulator 를 [[x86_64]] architecture 로 실행한다
  ```sh 
  arch x86_64 react-native run ios --simulator
  ```
- xcode 에서 Pods 의 `Build Settings` -> `Build Active Architectures Only` -> `Debug` -> `Any iOS Simulator SDK`에 `arm64` 값을 추가한다

# link
- [[ios]]
- [[bundler]]
- [[react-native]]
- [[cocoapods]]
- [[unity]]
- [[xcrun]]
- [[tart]]
- [[devide-app-by-development-env]]
