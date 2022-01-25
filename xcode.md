# xcode

## 빌드
### 원격 빌드
1. xcode 실행
2. iPhone 을 케이블로 연결
3. Window -> Devices and Simulators [shift + cmd + 2]
4. Device 화면 상단의 `Connect via netowrk` 를 선택

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
```
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
```
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

# related
- [[ios]]
- [[bundler]]
- [[react-native]]
- [[cocoapods]]
- [[unity]]