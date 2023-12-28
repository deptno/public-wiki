# fastlane
> 앱 배포과정을 자동화, 코드 사이닝,  배포등의 과정을 포함  
> 배포까지 자동화해야하기 때문에 개발자 등록을 마친 이후에 진행

## setup
> [[react-native]] 로 진행함  
> `fastlane` 설치는 프로젝트 루트에 각각에 대한 `fastlane init` 은 `/ios`, `/android` 에서 진행  
> system 으로 설치하는게 맞을수도 있을 것은 같은데 공식 문서에서 추천하지 않아서 ~~brew 설치~~ 하려했으나 [[react-native]] 프로젝트 루트에 있는 `Gemfile` 에 `gem 'fastlane'` 을 추가하여 `bundle install` 을 통해 설치

### 설치 방법
> [[macos]] 기준
- [ ] system ruby 사용, 공식 문서에서 권장하지 않음
- [ ] [[brew]] 사용, `brew install fastlane`
- [X] Gemfile 수정, [[react-native]] 에서 기본적으로 사용하고 있으므로 여기에서 함께 처리함

### [[testflight]] 사용시, 테스터 그룹 설정
- testflight 를 사용하는 경
- appstoreconnect
  - [X] 내부 테스팅 그룹 생성

### firebase app distribution 사용시, 테스터 그룹 생성
- firebase app distribution 콘솔
  - [X] 테스팅 그룹 생성
  ```ruby
  firebase_app_distribution(
    # 테스터 그룹 지정
    groups: '살지로',
  )
  ```

### ios
- [[react-native]] 프로젝트로 진행
  ```sh 
  cd ios
  bundle exec fastlane init
  ```
  - **Automate beta distribution to TestFlight** 선택
- 문서 `Podfile` 에 UTF-8 설정을 요구하고 있음, 설정하진 않음
  ```ruby 
  export LC_ALL=en_US.UTF-8
  export LANG=en_US.UTF-8
  ```
    + https://docs.fastlane.tools/getting-started/ios/setup/
- 아래 [[#인증]] 참조하여 진행
- 빌드 후 icon 에러, 아래 에러 참조
- testflight 에 올라간 것은 확인 되나 `No build_beta_detail included` [[#error]] 가 발생
  - appstoreconnect -> TestFlight -> 베타 정보 입력
- 성공

#### 인증
> 인증을 처리하지 않으면 진행되는 중간에 인터렉션을 해줘야한다  
> 인증 방법은 여러가지가 있으나 여기서는 fastlane 문서에서 권장하는 **App Store connect API Key** 를 사용
> **App Store Connect API Key** 를 권장하나 fastlane 에서 `produce`, `pem` action 혹은 tool 을 지원하지 못한다
  + https://appstoreconnect.apple.com/access/api

- 사용자 및 엑세스 -> App Store Connect API -> *App Store Connect API 액세스 요청* -> 바로 승인됨
  - `*.p8` 파일을 다운로드 받아 아래서 사용
- *API 키 생성* role 은 **앱 관리** 로 줘봄
  ```sh 
  [01:36:03]: ✅  Logging in with your Apple ID was successful
  [01:36:03]: Checking if the app '[BUNDLE_ID]' exists in your Apple Developer Portal...
  [01:36:04]: ✅  Your app '[BUNDLE_ID]' is available in your Apple Developer Portal
  [01:36:04]: Checking if the app '[BUNDLE_ID]' exists on App Store Connect...
  [01:36:04]: Looks like the app '[BUNDLE_ID]' isn't available on App Store Connect
  [01:36:04]: for the team ID '000000000' on Apple ID 'deptno@gmail.com'
  [01:36:04]: Would you like fastlane to create the App on App Store Connect for you? (y/n)
  ```
- `y` 를 누르면 생성해주기도 하는데 직접 생성하기로 한다
  - SKU 는 고유 아이디로 외부에 노출되지 않는다. `AAAA_0001` 이런식으로 생성이 가능
- 앱생성후에 `fastlane init` 을 이어가면 `Gemfile`과 함께 `fastlane/{App,Fast}File` 이 생성된다
- `bundle exec fastlane beta` achive 이 후 에러가 발생
  ```sh 
  [02:04:08]: ▸ Archive Succeeded
  # ...
  error: exportArchive: No profiles for '[BUNDLE_ID]' were found

  Error Domain=IDEProfileLocatorErrorDomain Code=1 "No profiles for '[BUNDLE_ID]' were found" UserInfo={IDEDistributionIssueSeverity=3, NSLocalizedDescription=No profiles for '[BUNDLE_ID]' were found, NSLocalizedRecoverySuggestion=Xcode couldn't find any iOS App Store provisioning profiles matching '[BUNDLE_ID]'. Automatic signing is disabled and unable to generate a profile. To enable automatic signing, pass -allowProvisioningUpdates to xcodebuild.}

  ** EXPORT FAILED **
  [02:04:19]: Exit status: 70

  # ...

  [02:04:20]: fastlane finished with errors

  [!] Error packaging up the application
  ```
  - **Provisioning Profile** 이 없어서 나는 에러로 디펄로퍼 콘솔에서 생성하면된다 **개발목적과 배포목적이 따로 필요**한데 여기선 [[testflight]] **배포** 목적이니 **Ad Hoc** 으로 생성
    - [ ] 생성 후 *Automatically manage signing* 을 체크 해제하고 *Download Profile* 을 선택해서 방금 생성한 프로파일을 선택
    - [X] 수동은 알아만 두고 자동으로한다
- *App Store Connect API Key*를 `lane` 안에 추가
  ```sh 
  app_store_connect_api_key(
    key_id: "[API KEY ID]",
    issuer_id: "[API KEY ISSUER]",
    key_filepath: "./[생성한 API KEY].p8",
    duration: 1200, # optional (maximum 1200)
    in_house: false # optional but may be required if using match/sigh
  )
  ```

### android
- [[react-native]] 프로젝트로 진행
  ```sh 
  cd android
  bundle exec fastlane init # 대충 기본만 하고 나머진 모두 생략 supply 도 하지 않음, fastlane/{AppFile,FastFile} 이 생성됨
  bundle exec fastlane test # fastlane/README.md 가 지금 생성됨
  ```
- `fastlane` 파일을 열어보면 `lane` 이 있어서 `bundle exec fastlane beta` 테스트
  ```sh 
  bundle exec fastlane [android] beta
  [05:46:33]: fastlane finished with errors

  [!] Could not find action, lane or variable 'crashlytics'. Check out the documentation for more details: https://docs.fastlane.tools/actions
  ```
  - `crashlytics` 에러가 발생 아무래도 [[../firebase|firebase]] 서비스를 이야기하는것 같은데 구글 계정 설정을 안해서 나는 것으로 보인다 이것부터 설정
  - 주석처리
- firebase app distribution 배포를 위한 `firebase app distribution` 플러그인 추가
  + https://firebase.google.com/docs/app-distribution/ios/distribute-fastlane?hl=ko
  ```sh 
  fastlane add_plugin firebase_app_distribution
  ```
- 계정 정보 추가
  + https://docs.fastlane.tools/actions/supply/
  - *구글 클라우드 콘솔*에서 *Google Play Android Developer API* 활성화
  - *구글 클라우드 콘솔*에서 *Service Account* 생성
    - fastlane-supply 와 같은 알아보기 쉬운 이름,  이메일은 해당 페이지에 있는 생성기 돌릴 것 -> **DONE**
  - 생성된 계정의 메뉴로 들어가 `Manage keys` 선택
    - ADD KEY -> Create New Key(JSON)
  - *구글 플레이 콘솔*에서 *Users and Permissions* 진입
    - Invite new users -> 위에서 생성한 서비스 어카운트 이메일 입력 -> Account Permissions 에서 admin 권한 후 초대
  ```sh
  bundle exec fastlane run validate_play_store_json_key json_key:[아까 다운로드 받은.json]
  ```
  - 성공 확인되면 AppFile `json_key_file("[아까 다운로드 받은.json]")` 로 수정
  - 이것으로 `crashlytics` 에러가 해결되지는 않음
  - `fastlane supply init` 
    - `[!] Google Api Error: Invalid request - Package not found: [package.name].` 에러가 발생
      - *구글 플레이 콘솔*  에서 앱을 아직 생성하지 않았고 한번은 업로드해야한다는 글들이 있음
        + https://stackoverflow.com/questions/69073389/fastlane-google-api-error-invalid-request-package-not-found-com-example
      - [[../wip|wip]]

## [[error]]

### Invalid username and password combination. Used 'XXXX@XXXX.com' as the username.
> 계정이슈
```sh
[10:19:46]: fastlane finished with errors

Looking for related GitHub issues on fastlane/fastlane...

Found no similar issues. To create a new issue, please visit:
https://github.com/fastlane/fastlane/issues/new
Run `fastlane env` to append the fastlane environment to your issue

[!] The request could not be completed because:
	Invalid username and password combination. Used 'XXXX@XXXX.com' as the username.
```
```sh
fastlane fastlane-credentials remove --username XXXX@XXXX.com
```

### fatal error: runtime: bsdthread_register error
```sh
▸ Running script 'Unity Process symbols'

❌  fatal error: runtime: bsdthread_register error


** ARCHIVE FAILED **


The following build commands failed:
        PhaseScriptExecution Unity\ Process\ symbols /Users/deptno/Library/Developer/Xcode/DerivedData/[APP_NAME]-afpmodexeqnqytfpispoaxqrjqft/Build/Intermediates.noindex/ArchiveIntermediates/[APP_NAME]/IntermediateBuildFilesPath/Unity-iPhone.build/Release-iphoneos/UnityFramework.build/Script-84294926AA792CC5C43FB36D.sh (in target 'UnityFramework' from project 'Unity-iPhone')
(1 failure)
[12:14:32]: Exit status: 65

+---------------+-------------------------+
|            Build environment            |
+---------------+-------------------------+
| xcode_path    | /Applications/Xcode.app |
| gym_version   | 2.199.0                 |
| export_method | app-store               |
| sdk           | iPhoneOS15.0.sdk        |
+---------------+-------------------------+

[12:14:32]: ▸ runtime.schedinit()
[12:14:32]: ▸   /usr/local/go/src/runtime/proc.go:492 +0xa1 fp=0x7ff7bfef50d0 sp=0x7ff7bfef5090 pc=0x102c651
[12:14:32]: ▸ runtime.rt0_go(0x7ff7bfef5100, 0x3, 0x7ff7bfef5100, 0x1000000, 0x3, 0x7ff7bfef6360, 0x7ff7bfef63ce, 0x7ff7bfef63da, 0x0, 0x7ff7bfef64b1, ...)
[12:14:32]: ▸   /usr/local/go/src/runtime/asm_amd64.s:175 +0x1eb fp=0x7ff7bfef50d8 sp=0x7ff7bfef50d0 pc=0x10540fb
[12:14:32]: ▸ Command PhaseScriptExecution failed with a nonzero exit code
[12:14:32]:
[12:14:32]: ⬆️  Check out the few lines of raw `xcodebuild` output above for potential hints on how to solve this error
[12:14:32]: 📋  For the complete and more detailed error log, check the full log at:
[12:14:32]: 📋  /Users/deptno/Library/Logs/gym/[APP_NAME]-[APP_NAME].log
```

`/Users/deptno/Library/Logs/gym/[APP_NAME]-[APP_NAME].log` 형태로 로그가 저장되니 여기서 확인하면 된다.
[[unity]] 항목 확인

### Error: Asset validation failed Missing required icon file
```sh 
[04:02:20]: [altool] 2023-12-27 04:02:20.553 *** Error: Asset validation failed Missing required icon file. The bundle does not contain an app icon for iPhone / iPod Touch of exactly '120x120' pixels, in .png format for iOS versions >= 10.0. To support older versions of iOS, the icon may be required in the bundle outside of an asset catalog. Make sure the Info.plist file includes appropriate entries referencing the file. See https://developer.apple.com/documentation/bundleresources/information_property_list/user_interface (ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx) (90022)
[04:02:20]: [altool]  {
[04:02:20]: [altool]     NSLocalizedDescription = "Asset validation failed";
[04:02:20]: [altool]     NSLocalizedFailureReason = "Missing required icon file. The bundle does not contain an app icon for iPhone / iPod Touch of exactly '120x120' pixels, in .png format for iOS versions >= 10.0. To support older versions of iOS, the icon may be required in the bundle outside of an asset catalog. Make sure the Info.plist file includes appropriate entries referencing the file. See https://developer.apple.com/documentation/bundleresources/information_property_list/user_interface (ID: e19b1052-837f-4d5a-b7a2-98a288cc04e5)";
[04:02:20]: [altool]     NSUnderlyingError = "Error Domain=IrisAPI Code=-19241 \"Asset validation failed\" UserInfo={status=409, detail=Missing required icon file. The bundle does not contain an app icon for iPhone / iPod Touch of exactly '120x120' pixels, in .png format for iOS versions >= 10.0. To support older versions of iOS, the icon may be required in the bundle outside of an asset catalog. Make sure the Info.plist file includes appropriate entries referencing the file. See https://developer.apple.com/documentation/bundleresources/information_property_list/user_interface, id=e19b1052-837f-4d5a-b7a2-98a288cc04e5, code=STATE_ERROR.VALIDATION_ERROR.90022, title=Asset validation failed, NSLocalizedFailureReason=Missing required icon file. The bundle does not contain an app icon for iPhone / iPod Touch of exactly '120x120' pixels, in .png format for iOS versions >= 10.0. To support older versions of iOS, the icon may be required in the bundle outside of an asset catalog. Make sure the Info.plist file includes appropriate entries referencing the file. See https://developer.apple.com/documentation/bundleresources/information_property_list/user_interface, NSLocalizedDescription=Asset validation failed}";
[04:02:20]: [altool]     "iris-code" = "STATE_ERROR.VALIDATION_ERROR.90022";
[04:02:20]: [altool] }

[04:02:20]: Application Loader output above ^
[04:02:20]: ERROR: [ContentDelivery.Uploader] Asset validation failed (90713) Missing Info.plist value. A value for the Info.plist key 'CFBundleIconName' is missing in the bundle 'ro.salji.app'. Apps built with iOS 11 or later SDK must supply app icons in an asset catalog and must also provide a value for this Info.plist key. For more information see http://help.apple.com/xcode/mac/current/#/dev10510b1f7. (ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
```
- 앱 아이콘에 없어서나는 에러로 구글에서 `app icon generator` 를 검색해서 아무거나 선택하면 될 것으로 보인다 사용한 링크는 아래추가
  + https://easyappicon.com

### No build_beta_detail included
- 베타에 대한 정보가 없어서 나는 것으로 추측
- appstoreconnect -> TestFlight -> 베타 정보 입력

### Missing authentication credentials
```sh 
Missing authentication credentials. Set up Application Default Credentials, your Firebase refresh token, or sign in with the Firebase CLI, and try again.
```
- **구글 클라우드 콘솔** -> 서비스 계정(**firebase 앱 배포** 권한이 있어야함) -> 배포에 사용되는 계정 ->  API KEY 생성된 것을 사용
- `fastlane/FastFile` 에서 인자로 인증키 위치 입력
  ```ruby 
  release = firebase_app_distribution(
    # ...
    service_credentials_file: '../[PROJECT-NAME]-[SERVICE_ACCOUNT_API_KEY].json',
  )
  ```

### the server responded with status 403
```sh 
the server responded with status 403
```
+ ~~https://stackoverflow.com/a/77438947~~
+ https://stackoverflow.com/questions/62385911/firebase-app-distribution-failed-to-fetch-app-information-403-the-caller-do
- 구글 클라우드 콘솔 ->  프로젝트 ->  IAM -> 서비스계정에 firebase 배포에 사용되는 계정 이메일 선택 -> **Firebase 앱 배포** role 추가
        
### the server responded with status 404
```sh 
the server responded with status 404
```
- [[firebase]] console -> Release & Monitor 에서 서비스가 시작된 것인지 확인(대시보드가 보여야함)
  - [[android]] / [[ios]] 를 **별도로 설정**해야하니 이 부분에 유의

### ios 다운로드 불가
+ https://firebase.google.com/codelabs/appdistribution-udid-collection?hl=ko#2
- 메일도 오고 테스트도 등록되었으나 다운로드 불가 
```txt 
기기가등록되었으며이제준비가끝났습니다
앱을테스트할준비가되면이메일이전송됩니다.
```
- 프로비저닝 이슈로 개인이 사용하는 경우라면 아래와 같이 인증서와 프로비저닝 설정을 추가하면 과정에서 필요한 것들이 자동으로 만들어지며 진행된다
- `fastlane/AppFile` 에 설정 확인
  ```ruby
  app_identifier("<your app's bundle identifier>")
  apple_id("<your Apple id>")
  ```
- `fastlane/FastFile` 에 설정 확인
```ruby
lane :lane_name do
  get_certificates()
  get_provisioning_profile(adhoc: true)
  build_app(export_method: "ad-hoc")
```
- 아래와 같이 처리된다
```sh 
[14:00:01]: Driving the lane 'ios firebase' 🚀
[14:00:01]: ------------------------------
[14:00:01]: --- Step: get_certificates ---
[14:00:01]: ------------------------------

+-----------------------------------------------------------------------------+
|                          Summary for cert 2.217.0                           |
+-------------------------+---------------------------------------------------+
| development             | false                                             |
| force                   | false                                             |
| generate_apple_certs    | true                                              |
| username                | deptno@gmail.com                                  |
| team_id                 | xxxxxxxxxx                                        |
| keychain_path           | xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx |
| skip_set_partition_list | false                                             |
| platform                | ios                                               |
+-------------------------+---------------------------------------------------+

[14:00:11]: Found the certificate xxxxxxxxxx (Bonggyun Lee) which is installed on the local machine. Using this one.
[14:00:11]: Verifying the certificate is properly installed locally...
[14:00:11]: Successfully installed certificate xxxxxxxxxx
[14:00:11]: Use signing certificate 'xxxxxxxxxx' from now on!
```
  + https://developer.apple.com
    - 인증서 생성된 것 확인 가능 TYPE: **Distribution**
    - 프로파일 생성된 것 확인 가능 TYPE: **Ad hoc**
    - 이후 배포되는 것들에대해서는 다운로드가 가능한 것이 확인된다
    - 추가적인 디바이스가 필요한 경우에는 **앱스토어 커넥트 콘솔** [[에서]] 디바이스 추가해서 진행
      - [[../udid|udid]] 등록확인
    - [ ] 여러 기기에서 빌드가 필요한 경우에는 [[match]] 필요 한것으로 보인다

## releated
- [[ios]]
- [[cert]]
- [[github]]
- [[unity]]
- [[firebase]]
