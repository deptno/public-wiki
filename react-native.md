# react-native

## 실행
### 설치
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

### 실행
```sh
[npx|yarn dlx|bun] react-native start [--reset-cache] # [[metro]] 실행 + 앱 런칭 옵션 제공
[npx|yarn dlx|bun] react-native run ios [--device "phonename"] # 특정 디바이스만
```

### 클린 빌드
```sh 
rm -rf node_modules
bun install # npm yarn pnpm 마찬가지
```
- [[ios]] cmd+shift+k 이후 빌드
- [[android]]
  ```sh 
  cd android/app
  ./gradlew clean build --refresh-dependencies
  ```

## 개발
### ios bundler identifier 변경
- ~~`ios/[APP_NAME]/Info.plist` 에서 `CFBundleIdientifer` 변경~~ 지금 보니 틀려보임 아래 확인
- `ios/saljiro.xcodeproj/project.pbxproj` 파일에서 `PRODUCT_BUNDLE_IDENTIFIER` 찾아서 일괄 변경

### android package name 변경
+ https://github.com/deptno/salji.ro/commit/720f51219b98fdf950fac402c172c380b55c31b6
- `android/app/build.grale` 에서 `namespace`, `applicationId` 를 변경
- 변경된 `applicationId` 에 **맞춰서** `android/src/{stage}/java/com,[MY_TLD]}/*` 형태로 디렉토리 이름을 변경

### firebase/*
- [[tbd]]

### firebase/messaging
+ https://rnfirebase.io/messaging/usage
+ [[firebase]
- app init 단계
```ts 
import messaging from '@react-native-firebase/messaging'

messaging().setBackgroundMessageHandler(async remoteMessage => {
  console.log('Message handled in the background!', remoteMessage)
})
```
- root component 
```ts
import messaging from '@react-native-firebase/messaging'
useEffect(() => {
    const unsubscribe = messaging().onMessage(async remoteMessage => {
      Alert.alert('A new FCM message arrived!', JSON.stringify(remoteMessage));
    });

    return unsubscribe;
  }, []);
```
- 상태에따른 핸들러  
  |        | active        | background, quit                |
  |--------|---------------|---------------------------------|
  | method | `onMessage()` | `setBackgroundMessageHandler()` |

### firebase-admin
- [[wip]]

### deeplink
+ https://kaumadiechamalka100.medium.com/how-to-implement-universal-link-app-link-in-react-native-a33eb6532612

#### [[iOS]] deeplink
1. `AppDelegation.mm` 을 수정한다
  ```objc
  #import <React/RCTLinkingManager.h>
  
  - (BOOL)application:(UIApplication *)application
     openURL:(NSURL *)url
     options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
  {
    return [RCTLinkingManager application:application openURL:url options:options];
  }
   ```
2. xcworkspace 파일을 열고 프로젝트 화면에서 **TARGETS** 선택 ->  info -> URL Types 를 추가한다
3. identifier, URL Schemes 는 사용할 프로토콜을 추가한다 `[프로토콜]://path`

#### [[iOS]] [[universal-link]]
> [[universal-link]] 는 [[apple]] 개발자 등록이 필요하고, [[universal-link]] 의 서브 주체인 웹서버가 필요하다

1. apple-app-site-association 파일 등록
  + https://developer.apple.com/documentation/xcode/supporting-associated-domains
  - `https://[DOMAIN.COM]/.well-known/apple-app-site-association` 와 같은 형태
  - *subdomain* 별 entitlement 추가 필요
2. https://developer.apple.com 에서 *Associated Domain* 활성화
3. *provision profile* 재생성
4. xcode 에서 *Signing Capabilities* 에 *Associated Domain* 추가 `applinks:` prefix가 필요하다
  - `applinks:[DOMAIN.COM]`
5. `AppDelegation.mm` - app 을 열 수 있도록 수정
  ```objc
  #import <React/RCTLinkingManager.h>
  
  - (BOOL)application:(UIApplication *)application
     openURL:(NSURL *)url
     options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
  {
    return [RCTLinkingManager application:application openURL:url options:options];
  }

  - (BOOL)application:(UIApplication *)application continueUserActivity:(nonnull NSUserActivity *)userActivity
   restorationHandler:(nonnull void (^)(NSArray<id<UIUserActivityRestoring>> * _Nullable))restorationHandler
  {
   return [RCTLinkingManager application:application
                    continueUserActivity:userActivity
                      restorationHandler:restorationHandler];
  }
  ```
6. `js` 코드에서 핸들링
  - app `quit`, `background` 상태일때
    ```ts
    import { Linking } from 'react-native'
    
    Linking.getInitialURL().then(url => {
      Linking.canOpenURL(url)
        .then([HANDLER])
        .catch([ERROR_HANDLER])
    })
    ``` 
  - app `foreground` 상태일때는 app 핸들링을 위해 app mount 이후 처리 필요
    ```ts 
    Linking.addEventListener('uri', event => {
      Linking.canOpenURL(event.url)
        .then([HANDLER])
        .catch([ERROR_HANDLER])
    })
    ```

#### [[android]] [[applink]]
> Android 6.0(API level 23) 이상에서 사용가능하다
> [[android]] [[applink]] 의 서브 주체인 웹서버가 필요하다
> Play App Signing 을 사용한다면 SHA256을 https://play.google.com/console 에서 확인하도록한다
> designate

1. `assetlinks.json` 생성
  - site domain, Application ID 가 필요한데 ID 는 `android/app/build.gradle` 에서 확인이 가능
  - Play App Signing 을 사용하고 있다면 해당 콘솔에서 SHA256 을 확인
  - [X] [[keytool]] 을 활용해서 SHA256 figerprint를 확인한다
    - `keytool -list -v -keystore [release.keystore]`
  - [ ] Android Studio 를 이용해서 생성할 수 도 있다
    - launch *Android Studio* -> Tools -> App Links Assistant -> Open Digital Asset Links File Generator
  - 최종 형태는 아래와 같다
    + https://developer.android.com/training/app-links/verify-android-applinks?hl=ko#web-assoc
    ```json 
    [{
      "relation": ["delegate_permission/common.handle_all_urls"],
      "target": {
        "namespace": "android_app",
        "package_name": "com.example",
        "sha256_cert_fingerprints":
        ["14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"]
      }
    }]
    ```
2. `assetlinks.json` 파일 등록
  - `https://[DOMAIN.COM]/.well-known/assetlinks.json` 로 서빙되어야한다
  - 테스트한다
    + https://developers.google.com/digital-asset-links/tools/generator?hl=ko
3. app 에서 받아 줄 수 있도록 앱링크 요청에 대한 처리를 한다
  + https://developer.android.com/training/app-links/verify-android-applinks?hl=ko#request-verify
  - `/android/app/src/main/AndroidManifest.xml` 수정
    ```xml 
    <activity ...>

      <intent-filter android:autoVerify="true">
          <action android:name="android.intent.action.VIEW" />
          <category android:name="android.intent.category.DEFAULT" />
          <category android:name="android.intent.category.BROWSABLE" />
          <data android:scheme="http" android:host="www.example.com" />
          <data android:scheme="https" />
      </intent-filter>

    </activity>
    ```
  - `android:host` 에 `localhost` 를 주입하는 경우 `port` 는 포함하지 않는다
- 추가적으로 딥링크 scheme 을 추가하고자한다면 그것도 함께 추가하면된다
  ```xml 
  <activity ...>

    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="[SCHEME_NAME]" />
    </intent-filter>

  </activity>
  ```
- 합쳐도 동작함
  ```xml 
  <activity ...>

    <intent-filter android:autoVerify="true">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="http" android:host="www.example.com" />
        <data android:scheme="https" />
        <data android:scheme="[SCHEME_NAME]" />
    </intent-filter>

  </activity>
  ```
- [[test]]
  - [X] `npx uri-scheme open --android '[SCHEME_NAME]://[PATH]'`
  - [ ] `adb shell am start -W -a android.intent.action.VIEW -d '[SCHEME_NAME]://[PATH]'`

## 설정
### neovim
- [[dap]]
- rn 파일 컨벤션 지원
  + https://github.com/deptno/nvim/commit/dda74cfcada336b0eb80cb84e84baf8e441dacf5
  + https://github.com/deptno/nvim/commit/7dc10ea5962e4cd2879399fd1fc8ca250e9648e2

### 환경별 빌드 설정
- [ ] [[env]], deeplink scheme 환경별로 분리되어야 테스트가 편하다
  + https://ricale.kr/blog/posts/210831-react-native-build-setting-for-envs/
  + https://velog.io/@dody_/RN-ios-뉴스-어플-헤드라잇-prod-dev앱-나눠서-관리하기-feat.-ios-Bundle-Identifier-ios-Scheme-ios-Build-Configuration

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

### warning: Building targets in manual order is deprecated
```sh 
warning: Building targets in manual order is deprecated choose Dependency Order in scheme settings instead, or set DISABLE_MANUAL_TARGET_ORDER_BUILD_WARNING in any of the targets in the current scheme to suppress this warning
```
- schema edit -> build -> dependency order

### Please make sure the template is valid
```sh 
✔ Downloading template
✖ Copying template
error Couldn't find the "/var/folders/yr/lb2jlrrd1fs7h6hn30n_ksvr0000gn/T/rncli-init-template-FoAnnF/node_modules/react-native/template.config.js file inside "react-native" template. Please make sure the template is valid. Read more: https://github.com/react-native-community/cli/blob/master/docs/init.md#creating-custom-template.
info Run CLI with --verbose flag for more details.
```
- `0.72.7` 에서 발생, `--npm` 옵션을 추가해서 해결

### View config getter callback for component `RNSScreen` must be a function (received `undefined`).
```sh 
 ERROR  Invariant Violation: View config getter callback for component `RNSScreen` must be a function (received `undefined`).

This error is located at:
    in RNSScreen
```
`RNSScreen` 문제는 아니고 상황이 상태에 따라 변한다. `react-native` 버전업을 하다가 리셋하고 다시 이전버전을 사용하면서 발생하게되었는데 조치했던 방법은 아래와 같다
- `bun install`, [[bun]] 사용중이었다
- `bundle exec pod deintegrate && bundle exec pod install`
- *Derived Data* 디렉토리 비움 by *DevCleaner*
- `git reset --hard`
- `bun start --reset-cache`

- 결과적으로 모두 안되었는데 `node_modules` 까지 지우고 다시 `bun install` 을 한 이후에 에러가 사라졌다.
- *중요* [[bun]] 사용시에 `isntall` 로 해결이 안되는 경우 `bun.lockb` 파일을 `node_modules`와 함께 지우고 클린 설치해야지 되는 경우가 있다
  + [[diary:2023-12-25]]

### Invariant Violation: View config getter callback for component `RNSScreenStackHeaderConfig` must be a function (received `undefined`).
- *중요* [[bun]] 사용시에 `isntall` 로 해결이 안되는 경우 `bun.lockb` 파일을 `node_modules`와 함께 지우고 클린 설치해야지 되는 경우가 있다
  + [[diary:2023-12-25]]

### Error: No safe area value available. Make sure you are rendering `<SafeAreaProvider>` at the top of your app.
- *중요* [[bun]] 사용시에 `isntall` 로 해결이 안되는 경우 `bun.lockb` 파일을 `node_modules`와 함께 지우고 클린 설치해야지 되는 경우가 있다
  + [[diary:2023-12-25]]

### DEVELOPER_ERROR
+ https://peerlist.io/blog/engineering/implementing-google-signin-in-react-native
- `@react-native-google-signin/google-signin` 패키지 사용하면서 발생
  + https://github.com/react-native-google-signin/google-signin
- [[android]] 에서만 발생
  - [[android]] [[oauth]] client key 는 *프로젝트*에 입력하지 않는다. [[android]] client id 를 발급받으면서 입력한 *SHA-1* 을 통해서 해당 디바이스가 요청하는 경우 인증으로 이어지게 된다(그래보임)
  - 문서에 존재하는 client id(아마도 web, ios)는 `configure` 의 인자로 입력하고 [[android]] client id 를 입력하지 않더라도 생성은 되어있어야한다
  + https://console.cloud.google.com/apis/credentials
    - 패키지 이름 `/android/app/src/main/java/` 이후 패스가 `.`으로 연결된 형태
    - *SHA-1* 은 [[keytool#사용]] 참조, debug 모드인 경우에는 기본적으로 생성되어있는 점 참고

### TypeError: Network request failed
- [[android]] 에서 발생
  - [[adb]] 이슈로 `adb reverse tcp:[PORT] tcp:[PORT]` 형태로 로컬웹서버를 사용한다면 해당 포트를 열어주면된다
- [[ios]] 에서 발생
  - react-native@0.73 을 기준으로 아래와 같은 설정이 되어있는 상태에서 설명
    ```xml 
    <key>NSAppTransportSecurity</key>
    <dict>
      <key>NSAllowsArbitraryLoads</key>
      <false/>
      <key>NSAllowsLocalNetworking</key>
      <true/>
    </dict>
    ```
  - `ip` 로는 로컬 네트워크에 접근이 가능하다. 문제는 [[android]] 의 `adb reverse tcp:3000 tcp:3000` 와 같은 기능이 없다는 것
  - 검색 결과 솔루션이 없는 것 같다. 인증 같은 경우는 테스트할 때 최종적으로 localhost 로 들어오게 되는데
    - 이를 local domain name 으로 처리하면 될 것 같기도 한데, 인증은 그냥 시뮬에서 하는걸로

### [!] The following Swift pods cannot yet be integrated as static libraries:
- [[iOS]] firebase 패키지 설치 이후에 발생
```sh 
bun add @react-native-firebase/{app,messaging} # 패키지 설치
cd ios
bundle exec pod install # 파드 설치 시작
# ... skip
[!] The following Swift pods cannot yet be integrated as static libraries:

The Swift pod `FirebaseCoreInternal` depends upon `GoogleUtilities`, which does not define modules. To opt into those targets generating module maps (which is necessary to import them from Swift when building as static libraries), you may set `use_modular_headers!` globally in your Podfile, or specify `:modular_headers => true` for particular dependencies.
```

- 에러 메시지와 마찬가지로 Podfile 마지막에 아래 줄을 추가로 해결한다
```txt
pod 'GoogleUtilities', :modular_headers => true
```
  + https://velog.io/@qkr135qkr/firebase를-iOS에-적용하면서-맞닦들인-문제들
  - 메인터네이너가 사용하지말라고함
    + https://github.com/invertase/react-native-firebase/discussions/6339#discussioncomment-4861993

### Module 'FirebaseCore' not Found
위에 Swift pods 문제를 해결하고 나서 xcode 통해 run 을 하게 되면 발생한다
```sh 
Module 'FirebaseCore' not found

$ bundle exec pod update
```
-  Swift pod 문제와 동일하게 아래 코드를 넣는다
```txt
pod 'FirebaseCore', :modular_headers => true
```
- 메인터네이너가 사용하지말라고함
  + https://github.com/invertase/react-native-firebase/discussions/6339#discussioncomment-4861993

### Cannot connect to metro. *warning*
- [[android]] 라면 [[adb]] 포트 확인, [[metro]] 포트인 `8081` 이 제대로 설정되었는지 필요
  - 혹은 *Change bundle location* 을 통해서 localhost 대신 개발 pc ip 로 접근

### No permission handler detected.
- 이슈 자체는 [[iOS]] 에서 `react-native-permissions` 의 `requestNotification(PERMISSIONS.ANDROID.POST_NOTIFICATIONS)` 하면서 발생
```txt
- Chceck that you are correctly calling setup_permissions in you Podfile.
- Uninstall this app, reinstall your Pods, delete your Xcode DerivedData folder and rebuild it.
```
- `react-native-permissions` 버전에 따라 갈리는데 `> @4` 인 겨우 `Podfile` 에 관련된 내용이 있는 `README.md` 참조
- `Podfile` 에서 permissions 설정을 하고 나면 `bundle exec pod install` 을 통해 해당 권한에 대한 파드를 설치하고 실행
  - **클린 빌드 필요**
    1. Product -> Clean Build Folder...
    2. Product -> Run

### [Error: Invalid notification (no valid small icon): Notification
- [[android]]
- `notifee` 에서 example 실행중에 발생
  ```sh 
  ERROR  [Error: Invalid notification (no valid small icon): Notification(channel=default shortcut=null contentView=null vibrate=null sound=null defaults=0x0 flags=0x10 color=0x00000000 vis=PRIVATE)]
  ```
  - 속성에 문제가 있는 것으로 이번 경우엔 아이콘 지우면 속성 제거하면된다.  이미지가 없다는 뜻인듯

### [Error: URL.pathname not implemented]
```ts 
const url = new URL(urlString)
const { pathname, searchParams } = url
```
- react-native 0.72.7 기준으로 *URL* 오브젝트가 정상 구현이 안된 것으로 보인다, 폴리필을 쓰던지,  알아서 처리하던지 해야한다.
  + **0.72.7** https://github.com/facebook/react-native/blob/v0.72.7/packages/react-native/Libraries/Blob/URL.js#L124-L233
  + **master** https://github.com/facebook/react-native/blob/8c0c860e38f57e18296f689e47dfb4a54088c260/Libraries/Blob/URL.js#L115-L222
- **polyfill** https://github.com/charpeni/react-native-url-polyfill

### Task :app:packageRelease FAILED
```sh 
> Task :app:packageRelease FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:packageRelease'.
> A failure occurred while executing com.android.build.gradle.tasks.PackageAndroidArtifact$IncrementalSplitterRunnable
   > SigningConfig "release" is missing required property "storeFile".
```
- **releaes** 빌드를 위해 `build.properties` 에 `*.keystore` 파일에 대한 정보를 패스워드 포함해서 주입이 필요함
- `gradle.properties` 에 선언한다,  참조하는 fastlane 과 페어로 동작
  ```sh 
  MYAPP_UPLOAD_STORE_FILE=release.keystore
  MYAPP_UPLOAD_KEY_ALIAS=release
  MYAPP_UPLOAD_STORE_PASSWORD=********
  MYAPP_UPLOAD_KEY_PASSWORD=********
  ```
  - 보안을 위해 `~/.gradle/gradle.properties` 에 넣어도 동작한다
### You must be registered for remote messages before calling getToken
```sh 
Error: [messaging/unregistered] You must be registered for remote messages before calling getToken, see messaging().registerDeviceForRemoteMessages().]
```
- react-native [[firebase]] [[fcm]] 라이브러ㅣ 사용시에 [[ios]] 에서 발생 에서 발생
- error `registerDeviceForRemoteMessages` 함수가 아니라 [[xcode]] signing -> capabilities 에 등록 필요
    
### 빌드된 버전에서 키자마자 죽는([[crash]])가 발생하느 경우
+ [[crash]]

#### [[android]]
- flipper 디바이스 연결된 상황에서 로그및 **Crash Repoter** 로 확인가능
- `react-native-config` 라이브러시 설정이슈 proguard 설정에 누락이 있었음
  + https://github.com/lugg/react-native-config/blob/bb3d813/README.md
  - 무지성으로 문서를 복사했는데 `package` 명에 대한 치환이 필요

#### [[ios]]
- [[tbd]]

## 필수 패키지 분석
```mermaid
flowchart LR
  react-native --> _push_ --> #react-native-firebase/messaging
  #react-native-firebase/messaging --> react-native-permissions
  #react-native-firebase/messaging --> #react-native-firebase/app
  #react-native-firebase/messaging --> AppDelegate.mm수정
  _push_ --> #notifee/react-native

  react-native --> _navigation_ --> #react-natigation/native --> react-native-screens
  #react-natigation/native --> #react-natigation/bottom-tabs
  #react-natigation/native --> #react-natigation/bottom-stack

  react-native --> _browser_
  _browser_ --> react-native-webview
  _browser_ --> react-native-inappbrowser-reborn

  react-native --> _view_
  _view_ --> react-native-safe-area-context

  react-native --> _oauth_
  _oauth_ --> #react-native-google-signin/google-signin
```

## 공식문서 분해
+  https://reactnative.dev

### .xcode.env 
+ https://reactnative.dev/docs/environment-setup#optional-configuring-your-environment
- `ios/` 폴더 밑
- `.xcode.env`, `.xcode.env.local`

### Workflow
#### Fast Refresh
잘 활용하기 위해서 지켜야할 컨벤션
- React Component 와 다른 변수등이 함께 `export` 되는 경우 정상동작을 보장하기 어려울 수 있다
- Class Component 의 state 는 fast refresh 시에 유지 되지 **않는다** -> Function Coponent 를 사용할 것
  - `// @refresh reset` 코드를 파일에 넣어두면 로컬 스테이트를 유지하는 대신 다시 로드된다
- use* 시리즈의 dependency 는 무시된다

#### Metro
+ [[metro]]
+ https://reactnative.dev/docs/metro
+ https://metrobundler.dev/docs/configuration/
- [[javascript]] bundler
- `metro.config.js` 사용

#### Symbolication
- 에러에 찍히는 로그가 bytecode 를 대신 `파일명:함수명` 을 출력
```sh 
# file
npx metro-symbolicate android/app/build/generated/sourcemaps/react/release/index.android.bundle.map < stacktrace.txt
# stdin
adb logcat -d | npx metro-symbolicate android/app/build/generated/sourcemaps/react/release/index.android.bundle.map
```
- sourcemap 은 사소한 변경에서 큰 offset 차이를 만들어낼 수 있으므로 crash 난 해당 해쉬의 코드를 참조해야만한다

#### Source Maps
- 번들링된 파일의 에러 위치를 찍어주는 대신 소스코드의 위치를 찍어준다
- `release` 모드에서 기본적으로 활성화 되며 `hermesFlagsRelease` 를 사용하는 경우 직접 명시해야 한다
- development build 는 bundle 을 기본적으로 만들지는 않기 때문에 이미 심볼을 가지고 있다
  - 번들을 만들어 하는 경우 `hermesFlagsRelease` 를 통해 활성화가 가능
- ios 에서 default 로 off 되어 있으며 `SOURCEMAP_FILE` 를 정의해야한다 문서 확인
  + https://reactnative.dev/docs/sourcemaps

#### Using Library
+ https://reactnative.directory
- native module 업데이트 후에는 리빌드가 필요 예) `bundle exec pod install` -> `yarn ios`

#### Using Typescript
- 기본으로 적용되고 방식은 tsc 로 타입체킹을 하는 것이며 babel 을 통해서 ts -> js 로 strip 한다

#### Upgrading to new versions
- 업그레이드는 쉽지 않다
- react-native-cli 혹은 업그레이드 헬퍼를 통해서 어떤 작업을 수행해야하는지 알 수 있다.
  + https://reactnative.dev/docs/upgrading 

### UI & Integration
+ https://yogalayout.dev/playground/
- camelCase css 를 사용한다
- 모든 core component 는 `style` property 를 가지고 있다
- style
  - javscript object
  - array of javscript object
  - return of StyleSheet.create
- react-native 는 `px` 등의 단위가 없다
- `flexDirection` 는 `column` 이 기본값

#### Images
+ https://reactnative.dev/docs/images
- 컴포넌트 파일과 같은 위치에 두면 장점이 많다
- `<Image />` 의 `source` 는 static import 하도록한다, - `require` 함수인자를 조합하지 않는다
  - 웹에서는 이미지 정보를 받기 전에 `0x0` 으로 렌더링을 하기때문에 layout shift 가 일어난다, 앱은 이를 의도적으로 구현하지 않았다
- `filename@[number]x.png` 컨벤션 사용, 디바이스 density 에 따라서 알아서 적용된다
- network iamge 의 경우 `width`, `height` 가 `style` 을 통해서 명시되어야한다
  + ATS 적용: https://reactnative.dev/docs/publishing-to-app-store#1-enable-app-transport-security
- base64 적용 가능
- [[iOS]]
  - 성능상의 이슈로 카메라롤 이미지가 섬네일 데이터와 함께 저장된다
    1. 이미지의 특정 사이즈를 요청하면 동일 사이즈의 섬네일이 존재하는경우 해당 이미지
    2. 아니면 최소 50% 이상 큰 다음 섬네일을 가져온다(blur 를 최대한 피하기 위함)
  - 수동으로 Cache Control 가능, 문서 확인
  - 전체 이미지 캐시용량등을 설정할 수 있다
- 이미지 디코딩은 메인 스레드가 아닌 다른쓰레드에 진행된다
  - [ ] webview 에서 하게되면 이문제가 발생할 수 있을 것으로 생각된다

#### Interaction
##### Handling Touches
- `Button` 외에도 더 디테일한 컨트롤을 제공하는 `Touchable` 이 존재한다
- `Button` 은 `Touchable` 의 자주쓰이는 prop 을 fix 해둔것으로 이해된다

##### Navigation between screens
+ https://github.com/react-navigation/react-navigation

##### Animation
- [ ] TODO:

##### Gesture Responder System
- [ ] TODO:
- View 를 Response 로 만들 수 있음
- high level 인터페이스는 `PanResponder` 참조

#### Connectivity
##### Networking
- `fetch`, `WebSocket` 을 지원
- cookie 베이스 인증에 이슈가 있음
  + https://reactnative.dev/docs/network#known-issues-with-fetch-and-cookie-based-authentication
  - [[android]] 동일 헤더 네임 무시됨
  - [[iOS]] 302 시에 set-cookie 제대로 동작 안함
  - 쿠키 인증 자체가 *unstable*

##### Security
+ https://reactnative.dev/docs/security
- environment variable 을 serverside 와 헷갈리지말 것 app 내부에서 모두 접근가능해서 위험
- 중요한 정보라면 server 를 통해서 접근(app에서만 접근할 수 있는 방식으로)
- storage 에 저장하는 것 자체가 위험, 꼭 저장할 필요가 있는지
- sentry 등의 모니터링 시스템에 의해서 노출되는 것을 조심
- Async Storage(app의 local storage) 에 `token`, `secret` 을 저장하지 말 것
- secure storage 로 플랫폼별로 가지고 있는 keychain, key store 등을 이용가능
  - bridge 가 필요하니 라이브러리 참조할 것, 문서 참조
- oauth
  - app 에서는 app:// 의 deeplink 스키마를 신뢰할 수 없기 때문에 PKCE 를 사용한다
- ssl pinning
  - https 는 안전하지만 위조한 인증서를 사용해서 무력화가 가능
  - 이를 막기 위해 안전한 인증서 목록을 클라이언트에 심어서 자체인증된 인증서를 거부
  - 이를 사용하는 경우 인증서는 1,2 년마다 만료됨을 인지하고 지속적인 업데이트가 필요

#### Inclusion
- [ ] TODO:

### Debugging
#### Debugging Basics
- `REACT_DEBUGGER` 환경 변수를 통해서 iOS  에서도 chrome debugger 를 사용할 수 있을 수 있을지

#### React Developer Tools
- `Show Inspector` 메뉴가 standalone 버전의 React Native Inspector 와 만나면 동작한다

#### Native Debugging
```sh 
npx react-native log-android
npx react-native log-ios
```

### Testing
- [ ] TODO:

### Performance
- `console.log` 등은 퍼포먼스 하락을 불러온다 babel plugin 을 통해서 제거하던지 하자
- `ListView` 대신 `FlatList`,  `SectionList` 를 사용
  - 느리면 `getItemLayout` 확인
  - 느리면 `rowHasChanged` 확인
- Animation
  - `useNativeDriver: true` 를 주지 않는이상 JS thread 에서 동작하게됨
  - LayoutAnimation 은 JS thread 영향을 받지 않지만 중간에 멈추지 못하는 점을 유의
  - `ListView` 의 경우 `rowHasChanged` 메서드가 제공되고 있는지 확인
- Image
  - [[iOS]] 에서 width, height 를 바꾸는건 매우 비용이 크다 -> `transform: [scale]` 을 활용
- Touchable
  - `onPress` 등의 handler 에서 너무 많은 작업을 하는 경우 -> `requestAnimationFrame` 으로 감싸서 실행

#### Speeding up your Build phase
+ https://reactnative.dev/docs/build-speed
- [ ] [[android]] 에서 사용하는 아키텍처의 ABI 만 생성하도록 `npx react-antive run-android --active-arch-only` 플래그 추가
- [ ] native cache, ccache, sccache 를 이용 문서 참조

#### Speeding Up CI Builds
+ https://reactnative.dev/docs/speeding-ci-builds
- disable flipper for iOS `NO_FLIPPER=1 bundle exec pod install --project-directory=ios` 를 통해 가능
- flipper 라이브러리가 추가로 존재하는 경우 `react-native.config.js` 파일을 통한 추가 정리가 필요하다 문서 참조

#### Optimizing Flatlist
- `Flatlist` props
  - `removeClippedSubviews`: 뷰포트 밖에 있을때 view hierachy 에서 detach(remove 가 아니다)
  - `maxToRenderPerBatch`: 배치당 한번에 그려질 아이템 수
  - `updateCellsBatchingPeriod`:  배치의 간격 interval 정도로 이해
  - `initialNumToRender`:기 초기에 그려질 크
  - `windowSize`: 메모리에 그려질 크기 `viewport` 의 배수
- `List` items
  - 가볍고 기본적은 컴포넌트 사용
  - use `react-native-fast-image`, for caching + optimizing
  - use `getItemLayout`,  리스트가 동적으로 layout을 계산할 필요 없도록 제공
  - use `keyExtractor`, for cacheing + traciking re-ordering
  - avoid anonymous function on `renderItem`, 대충 파일 분리해서 re-creating 을 피하란 말

#### Ran Bundles and inline requires
- [ ] TODO:

#### Profiling
- [ ] TODO:

#### Profiling with Hermes
- [ ] TODO:

### JavaScript Runtime
#### Javascript Environment
- [[hermes]] 가 `enable` 된 상태라면 해당 엔진을 사용하고 아닌 경우 [[javascript-core]] 를 사용
- [[chrome]] debugger 가 연결되면 [[chrome]] 의 [[v8]] 을 debugger 로 사용
- 비슷하지만 조금 다른점도 있어 특정 플랫폼에 기대지않는게 최고, 특히 Date 등
  - [[android]] 와 [[v8]] debugger 간의 차이로 Date 디버깅시 필요
  ```sh 
  adb shell "date `date +%m%d%H%M%Y.%S%3N`"
  ```
  - 디바이스의 경우 root access 가 요구된다
- babel 을 통해 트랜스파일되어 최신 문법 사용 가능
- polyfill 도 대충 포함되어있따는 말로 이해

#### Timers
| fn                                          | 설명                                                                                              |
|---------------------------------------------|---------------------------------------------------------------------------------------------------|
| setTimeout(fn, 0)                           | 모든 frame 이 flush 된 이후 실행                                                                  |
| requestAnimationFrame(fn)                   | 최대한 빠르게 실행                                                                                |
| setImmediate(fn)                            | 현재 js 실행 블록이 끝나고 실행, 중첩 실행시 비동기로 동작하지 않음, 비동기 실행시는 promise 사용 |
| InteractionManager.runAfterInteractions(fn) | 롱텀 동기 실행 타스크를 위함 + 현재 애니메이션을 블락하지 않음                                    |

- `InteractionManager.createInteractionHandle` 을 통해 반환된 handle은 queue 에 있는 task 를 취소하는데 사용 가능

#### Using Hermes
- [[hermes]] 참조
- `yarn ios --mode Release` 를 통해서 bytecode 로 빌드 가능 앱이 디바이스에서 뜨는 속도를 향상시킨다
- [[chrome]] devtool 을 이용해서 [[hermes]] 에서 디버깅 가능, *Remote JS Debugging* 메뉴와는 완전히 다른 내용

### Native Modules
- [ ] TODO:

### Native Components
#### Native Ui Components
- @deprecated

#### Direct Manipulation
- `setNativeProps`: 애니메이션과 제스쳐  응답과 같이 같이 특수한 상황에서 렌더 트리 계층을 건너띄어야할때만 사용
- 기본적으로 다이렉트로 멀리있는 자식 컴포넌트에 `props` 를 직접 주입하는 방법
- [ ] TODO: 필요할 떄 상세 참조

### The New Architecture
- as-is
  - 데이터가 불필요하게 비동기로 전달된다
  - 싱글스레드
  - bridge 라는 컴포넌트를 사용해서 데이터를 serialize/deserialize 하는 형태로 사용했다
- to-be
  - 동기 실행
  - 동시성(다른스레드에서 동작하는 네이티브 함수를 여러번 콜함으로써 얻는 이득의 의미일 듯)
  - serialize/deserialize 비용 제거

#### New Architecture
- Fabric: RST 가 JSI 를 통해 호스트에서도 C++로 존재할 수 있는 방식 Render Pipeline 에서는 Render, Commit 이 해당

##### Render, Commit, and Mount
- Render Pipeline
  - 단계는 쓰레드 동립적일 수 있다
  - Render: React Element Tree(JS) -> React Shadhow Tree(C++)
  - Commit: React Shadhow Tree 가 완전히 생성된 후 RET, RST 를 *next tree* 로써 mount 준비한다
    - `onLayout`from *core*
  - Mount: RST + Layout Caculation -> Host View Tree
    - `onChange` from *host platform*
###### Phase 1. Render
- React Shadow Node 는 Recat Composite Component 가 아닌 Host Component 와 매칭된다
- [ ] TODO:

#### Createing a New Architecture App
- RN@0.68 이상
- [[iOS]] 환경변수 `RCT_NEW_ARCH_ENABLED=1 bundle exec pod install` 을 통해서 가능
- [[android]] 환경변수 혹은 `android/gradle.properties` 에서 설정을 통해 가능
- 확인 시작 로그의 `fabric` 을 통해서 확인 가능
```sh 
 LOG  Running "APP NAME" with {"fabric": true}
```
- new architecture library 는 fabric component, turbo module 로 명명된다
- [ ] TODO: 현재 `react-native-inappbrowser-reborn` 라이브러리가 fabric 을 지원하지 않아 여기서 pending

### 공식 문서에 언급된 라이브러리들
- react-navigation
- react-native-keychain 관련
- react-native-fast-image

### 공식 문서에 언급된 커맨드
```sh 
npx react-native upgrade
```

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
- [[react-navigation]]
- [[fastlane]]
- [[webview]]
+ https://www.reactnative.express/exercises
