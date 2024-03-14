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

## link
- [[google]]
- [[google-adsense]]
- [[react-native]]
