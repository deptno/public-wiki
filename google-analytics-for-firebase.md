# google-analytics-for-firebase

## 관련 문서
+ https://firebase.google.com/docs/analytics?hl=ko
+ https://rnfirebase.io/analytics/usage

## 사용
> 아래 내용은 공식문서 기준 [[swift]] 코드임을 감안
```sh
Analytics.logEvent('EVENT_NAME', [PARAMETER])
Analytics.setDefaltEventParameters([DEFAULT_PARAMETER])
Analytics.setUserProperty(food, forName: "favorite_food")
```

### adsupport
- IDFA 활성화가 요구됨
- [[android]] 는 기본적으로 지원되나 [[ios]] 에서 사용하려는 경우 추가 설정이 필요
  + https://rnfirebase.io/analytics/usage#device-identification
- 연령, 성별, 관심분야 + ID(IDFA) 가 자동 수집됨
+ https://firebase.google.com/docs/analytics/user-properties?hl=ko&platform=ios

### [[xcode]] 디버그 콘솔에서 이벤트 보기
+ https://firebase.google.com/docs/analytics/events?hl=ko&platform=ios
- xcode 명령줄 인수에 `-FIRDebugEnabled`를 포함
  - [[xcode]] -> edit scheme -> run -> Arguments -> Arguments Passed On Launch
- 앱의 로깅 이벤트는 한시간 취합후 일괄 업로드, 디버그 모드를 사용하면 즉시 볼 수 있음
- firebase console 의 debug view 에서 확인 가능

## webview
- 네이티브로 메시지 전송 후 네이티브에서 처리

## event
### screen_view
> [[react-native]] 에서는 싱글 ViewController([[android]]: Activity) 로 구성되기 때문에 네이티브 이벤트로 추적은 의미가 별로 없어 수동 추적이 필요하다
- 화면 전환시 일어난다
- `firebase_screen_class` 파라메터를 통해 확인할 수 있다
- 다만 이걸로는 [[react-native]] 프로젝트 진행시 내가 원하는 화면 이름을 보는 것은 아닌것같다
  - webview 인지, 일반화면인지 정도가 구분되는 것으로 보인다, [[ios]] 레벨에서 구분
- 기본 화면 조회수 추적 금지 사용가능
  + https://firebase.google.com/docs/analytics/screenviews?hl=ko&platform=ios
  - [[react-native]] 사용시
    + https://rnfirebase.io/analytics/usage#disable-screenview-tracking
- 수동 사용
  - [[react-native]] react-navigation 을 사용중인 경우 아래내용을 추가
    + https://rnfirebase.io/analytics/screen-tracking
    + https://reactnavigation.org/docs/screen-tracking/

### ad_impression
- 광고를 볼때마다 ad_impression 이벤트를 로깅해야함
- [[google-admob]] 을 연동하는 경우에 자동으로 전송됨
- **note** AppLovin 및 ironSource 는 노출마다 수익을 주는 구조인가 봄 노출 시마다 메시지 발송

## 사용자 ID 설정
+ https://firebase.google.com/docs/analytics/userid?hl=ko
- **서드파티** 에서 사용자 식별이 가능한 email, 주민등록번호 등이 사용자 ID 로 사용되어서는 안된다
- 

## link
- [[firebase]]
- [[google-analytics]]
- [[react-native]]
- [[xcode]]
