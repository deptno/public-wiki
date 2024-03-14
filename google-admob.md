# admob
> 인앱 광고를 위한 adsense

## 설치
+ https://docs.page/invertase/react-native-google-mobile-ads
- `react-native-google-mobile-ads` 설치
- [[android]] 앱 릴리즈 전에 앱이 광고를 포함하고 있음을 선택해야한다
- [[ios]] `podfile` 수정이 필요하다 위 사이트 참고
- https://apps.admob.com 에서 앱 -> 앱설정 -> **앱ID** 를 프로젝트 root `app.json` 에 추가
  - `android_app_id`
  - `ios_app_id`
  - `sk_ad_network_items` 설정 추가 IDFA 비활성화일때도 앱 설치 추적지원을 위함
    + https://developers.google.com/ad-manager/mobile-ads-sdk/ios/3p-skadnetworks?hl=ko
    - 광고로 설치된 앱을 말하는 것으로 추측
  - `user_tracking_usage_description` [[ios]] att 요청에 대한 설명 요구됨
- 코드레벨
  - `setRequestConfiguration`, 어린이대상으로하는 앱이라면 광고를 수신하기 전에 해당 설정을 초기화가 우선되어야한다
  - `initilize()` 필요
  - [[ios]] PERMISSIONS.IOS.APP_TRACKING_TRANSPARENCY 요청이 필요
  - 이후는 ad load -> show

## 설정
- firebase 콘솔에서 admob 시작하기
- 스토어에 배포된 앱 선택으로 진행
- 스토어에 등록된 마케팅 웹페이지에 `app-ads.txt`  추가해서 서빙
- `app.json` 말고 개별 광고에는 광고 유닛 아이디가 들어가기 때문에 admob 콘솔에서 광고 생성

## react-native
> 실제 unit id 가 사용될 때는 bundle id 와 일치해야함(프로덕션 빌드만 가능하다는 뜻)
- `__DEV__` 는 로컬 개발환경을 구분
- debug 환경을 릴리즈 빌드하면 실제 unitId 등이 들어가게되고 이런 경우에는 광고가 노출되지 않음
- production 환경으로 빌드시 나오는걸 확인할 수 있음
- 만약 dev 와 같은 스테이지 빌드를 하고자 한다면 그냥 `TestIds.*` 을 넣어버리던지 실제 unit id 를 넣고자한다면 테스트 디바이스에 넣어야하는 것으로 추측된다. 난 귀찮아서 전자 처리, 안그러면 아래와 같은 네이티브 로그를 확인할 수 있다
  ```sh 
  <Google> To get test ads on this device, set: 
  Objective-C
    GADMobileAds.sharedInstance.requestConfiguration.testDeviceIdentifiers = @[ @"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" ];
  Swift
    GADMobileAds.sharedInstance().requestConfiguration.testDeviceIdentifiers = [ "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" ]
  ```

## link
- [[google]]
- [[google-adsense]]
- [[react-native]]
