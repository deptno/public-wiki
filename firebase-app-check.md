# firebase-app-check
+ [[diary:2024-01-01]]

## 컨셉
- 디바이스에 인증개념을 도입해서 해당 앱으로부터 정상적으로 발송된 트래픽인지를 확인하고 아닌경우 거부한다
- 특정 [[firebase]] 서비스들과 통합된
  - firebase 의 특정 서비스를 사용할때 앱에서 발송된것이 확인된 트래픽만 허용
  - firebase 의 특정 서비스란 디비나 함수등을 사용하는 서비스
- 자체 서버에서도 사용할 수 있다 [[#커스텀 서비스(자체 백엔드)와의 통합]] 참조

## 커스텀 서비스(자체 백엔드)와의 통합
+ https://firebase.google.com/docs/app-check/web/custom-resource?hl=ko
+ https://firebase.google.com/docs/app-check/custom-resource-backend?hl=ko
- 요약하면 발송 주체에서 발행한 토큰을 헤더에 실어서 보내고 서버에서는 `firebase-admin-sdk` 를 통해서 검증하는 방식

### 한계
- 토큰은 api call 당 발행되는 토큰이다
- 발행된 토큰을 캡쳐해서 어뷰징을 하는 경운에는 컨셉이 무너진다
- 한계 극복을 위해 **재생보호** 라는 컨셉이 존재하며 한번 사용한 토큰이 재사용되는지를 확인하기 위해 서버에 다녀오는 방식이다
  - 이 경우 매 요청에 대해서 [[firebase]] 에 다녀와므로 지연이 추가된다
  - 공식문서에서도 지연에 민감한 곳에서는 사용하지 말기를 추천한다

## 도입시 고려할 점
- 앱을 새로개발하는 것을 기준으로 도입이 늦어지면 도입전 트래픽이 구버전 트래픽으로 남아있는 이상 트래픽이 막히는 이슈가 발생한다
- 강업등을 고려해야하므로 초기에 도입해두고 *활성화 여부는 사용 여부이므로* 서버에서 결정하는 것이니 추후에 결정해도 될 것으로 생각된다

## 도입
> `@react-native-firebase/app-check` 기준

### [[android]]
+ https://firebase.google.com/docs/app-check/android/play-integrity-provider?hl=ko#project-setup
- https://play.google.com/console -> 앱 진입후-> 출시 -> 앱 무결성-> Play Integrity API -> 설정 -> 구글 클라우드 프로젝트와 연결
- https://cosole.firebase.google.com -> App Check -> Play Integrity -> SHA-256 제공
  - SHA-256 는 [[keytool]] 참조
- [[#appCheck/token-error]] 에러발생
  - 커스텀 서버에서만 사용할 생각이었는데 너무 많은 에러가 나고 있어서 기록 남기고 rollblack [[diary:2024-01-02]]

### ios
+ https://firebase.google.com/docs/app-check/ios/app-attest-provider?hl=ko#project-setup
- `pod install`
- open [[xcode]] `.xcworkspace` -> Signing Capabilities -> Attest 추가
  - `saljiro.entitlements` 에 값을 `production` 으로 변경
- 400
  ```sh 
  ERROR  [Error: [appCheck/token-error] The operation couldn’t be completed. Too many attempts. Underlying error: The operation couldn’t be completed. The server responded with an error:
   - URL: https://firebaseappcheck.googleapis.com/v1/projects/saljiro/apps/1:953228161449:ios:1ba4b794206e1b315194b8:exchangeDeviceCheckToken
   - HTTP status code: 400
   - Response body: {
    "error": {
      "code": 400,
      "message": "App not registered: 1:953228161449:ios:1ba4b794206e1b315194b8.",
      "status": "FAILED_PRECONDITION"
    }
  }
  ]
  ```
  - AppDelegate.mm
    ```objc
    // @react-native-firebase/app-check 요구사항
    @interface YourAppCheckProviderFactory : NSObject <FIRAppCheckProviderFactory>
    @end

    @implementation YourAppCheckProviderFactory

    - (nullable id<FIRAppCheckProvider>)createProviderWithApp:(nonnull FIRApp *)app {
      return [[FIRAppAttestProvider alloc] initWithApp:app];
    }

    @end

    @implementation AppDelegate

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
    // @react-native-firebase/app-check 요구사항 +
    YourAppCheckProviderFactory *providerFactory = [[YourAppCheckProviderFactory alloc] init];
    [FIRAppCheck setAppCheckProviderFactory:providerFactory];
    // @react-native-firebase/app-check 요구사항 -
    ```
  - 실기기는 여기서 됨
  - https://console.firebase.google.com -> app check -> 앱 ->  디버그 토큰 생성
    - 이걸 추가안해도 debug 에서 에러가 나질 않아 제거함
  - [[xcode]] -> edit schema -> arguments -> `-FIRDebugEnabled` 추가
    - 이걸 추가안해도 debug 에서 에러가 나질 않아 제거함

## [[error]]
### [appCheck/token-error]
```sh 
[Error: [appCheck/token-error] com.google.android.play.core.integrity.IntegrityServiceException: -9: Integrity API error (-9): Binding to the service in the Play Store has failed. This can be due to having an old Play Store version installed on the device.
 (https://developer.android.com/google/play/integrity/reference/com/google/android/play/core/integrity/model/IntegrityErrorCode.html#CANNOT_BIND_TO_SERVICE).]
```
- custom 인증을 위해서 사용하기 위해서 너무 많은 공수가 들어가고있다

## link
- [[firebase]]
- [[react-native]]
