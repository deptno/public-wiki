# devide-app-by-development-env|앱 개발 환경 분리
> 앱 개발시에 환경에따라 마치 다른 앱처럼 각각 설치하는 것을 목표로한다  
> 그 이후에 환경에 따른 환경변수를 적용한다  
> [[fastlane]] 적용까지를 목표로 한다  
> 이 페이지는 프로젝트 최초셋업아니고 마이그레이션을 전제로 분리에만 포커싱한다

## [[todo]]
- [[todo]] firebase 완전 분리
  - fcm 까지 분리할 계획이라면 [[apns]] 연결등의 추가적인 작업이 필요하다 현재는 필요없다
  - [[ios]] 생성후에 GoogleService-info.plist 를 적용하지 않았다
    - [[fcm]] 이 다른 bundle identifier 가 사용된다는 의미로 production 계정과 꼬일 수 있다는 점 정도 염두

## [[ios]]
+ https://github.com/deptno/salji.ro/commit/53c03555e6faa3dc9e7b015ec1c4b6a05b609957

### bundle identifier 분리
- bundle identifier 에 의해 앱이 구분되므로 이를 분리해야한다
- bundle identifier 를 분리하기 위해서는 먼저 **configuration** 을 생성해야한다
- **configuration** 이 생성되면 이에 따라 다른 값을 읽는 변수를 지정할 수 있고 bundle identifier 도 마찬가지로 달리 지정이 가능하다

### configuration 생성
- [[xcode]] 에서 프로젝트를 열고 **project** -> *info* 선택 하면 **Configurations** 가 나오고 여기에 추가를 한다
- [[react-native]] 프로젝트를 기준으로 이미 *Debug*, *Release* 가 존재한다.
- 여기에 **Dev** 를 추가한다.
  - **Dev** 는 개발 서버를 바라보는 개발버전의 릴리즈 빌드

### 변수 설정
- [[xcode]] 에서 프로젝트를 열고 **project** -> **target** -> *Build Settings* 선택한다
  - *Packaging* -> `Product Bundle identifier` 을 선택하여 **configuration** 에 따라 각각 다른 번들 이름을 주도록 한다
- **project** -> **target** -> *Build Settings* 화면에서 `+` 버튼을 눌러 **Add User Define Setting** 을 눌러 변수를 추가한다
  - 변수명은 `DISPLAY_APP_NAME` 으로 하였고 각각 이름을 달리하여 설치한 앱이 각각 어떤 환경을 나타내는지 구분할 수 있도록 한다

### 변수 설정
- [[xcode]] 에서 프로젝트를 열고 **project** -> **target** -> *Build Settings* 선택한다
  - *Packaging* -> `Product Bundle identifier` 을 선택하여 **configuration** 에 따라 각각 다른 번들 이름을 주도록 한다
- **project** -> **target** -> *Build Settings* 화면에서 `+` 버튼을 눌러 **Add User Define Setting** 을 눌러 변수를 추가한다
  - 변수명은 `DISPLAY_APP_NAME` 으로 하였고 각각 이름을 달리하여 설치한 앱이 각각 어떤 환경을 나타내는지 구분할 수 있도록 한다

### [[firebase]]
- firebase 콘솔의 프로젝트 관리에 들어가서 추가된 [[ios]] bundle identifier 마다 앱을 생성
- firebase app distribution 에서 해당 앱을 선택하고 시작하기를 눌러줘야지 배포시에 에러가 나지 않는다k
- 
### [[fastlane]]
- [[fastlane]] 을 통해 배포할때 두가지를 이용하고 있었다 여기서는 [[firebase]] app distribution 을 다룬다
  - [ ] testflight - 분리하지 않는 이유 [[diary:2024-01-15]] 참고
  - [X] [[firebase]] app-distribution
- `Fastfile` 에 아래와 설정이 추가된다
  ```ruby
    build_app(
      # ...
      configuration: "[CONFIGURATIONS]", # 생성한 configuration 적용
      export_options: {
        provisioningProfiles: {
          "[BUNDLE_IDENTIFIER]" => "[PROFILE_NAME]", # 생성한 identifier 에 따른 profile 적용
        },
      },
    )
    firebase_app_distribution(
      # ...
      app: '[FIREBASE_APP_ID]', # bundle identifier 마다 app id 가 다르다
    )
  ```
  - identifier 가 하나일떄는 자동으로 잘 동작을 해줬으나 여러개가 되고 난 이후로 찾지 못해서 수동으로 생성 후 매칭해줬다
  - bundle_identifier 에 따라 [[firebase]] 앱을 각각 생성해야하니 이 부분을 적용한다
  - `Domain=IDEProfileLocatorErrorDomain Code=4` 에러가 발생하면 [[#error]] 참고한다

## [[android]]
- [[tbd]]

## [[error]]
### Domain=IDEProfileLocatorErrorDomain Code=4
```sh 
Error Domain=IDEProfileLocatorErrorDomain Code=4 "No "iOS Ad Hoc" profiles for team 'Bonggyun Lee' matching '[PROFILE_NAME]' are installed." UserInfo={IDEDistributionIssueSeverity=3, NSLocalizedDescription=No "iOS Ad Hoc" profiles for team 'Bonggyun Lee' matching '[PROFILE_NAME]' are installed., NSLocalizedRecoverySuggestion=Install a profile (by dragging and dropping it onto Xcode's dock item) or specify a different profile in your Export Options property list.}
```
- [[fastlane]] 에서 앱 배포가 끝나는 시점에 발생했다.  [[fastlane]] 시작부분의 로그를 보니 다른 *identifier* 에 대한 인증서를 매칭하고 수동으로 매칭했다
- 수동으로 만들면서 이름에 띄여쓰기와 한글을 넣었는데 이 부분이 문제가 되는가 싶어서 띄여쓰기 와 한글을 제외하고 단순하게 만들어서 넣었더니 매칭되었다

### Unable to load contents of file list
```sh 
❌  error: Unable to load contents of file list: '.../ios/Pods/Target Support Files/Pods-saljiro/Pods-[PROJECT_NAME]-resources-Dev-output-files.xcfilelist'
```
- configuration 을 변경하면 그에 따라 파일을 참조하는데 해당 파일이 없다는 에러
- 해당 파일은 `bundle exec pod install` 생성된다, 생성 후 다시 시도

### `firebase_app_distribution`
```sh 
Invalid request
```
- [[fastlane]] `firebase_app_distribution` 스텝에서 발생했다.
- https://firebase.console.google.com 에 접속해서 새로 생성한앱의 app distribution 프로젝트 생성해준다

## link
- [[react-native]]
- [[firebase]]
- [[fastlane]]
- [[xcode]]
- [[fcm]]
