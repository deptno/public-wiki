# push-notification
- app: active
  - 수신: onMessage
- app: active 가 아닌경우
  - 수신: setBackgroundMessageHandler
  - ~~getInitialNotification~~ `@notifee/react-native` 사용시 `notifee.getInitialNotification`
- remote notification 이 눌린경우
  - 수신: ~~onNotificationOpenedApp~~ `@notifee/react-native` 사용시 `notifee.onForegroundEvent` - `type: 'PRESS'`
- 서버에서 토큰이 변경된 경우
  - 수신: onTokenRefresh

## [[fcm]] - push-notification 흐름도
### 딥링크 처리까지의 큰 흐름
```mermaid
flowchart
  style notifee fill:#bbf
  style firebase fill:#f66
  style react-native fill:#e91

  fcm --payload--> onMessage
  fcm -.payload.-> setBackgroundMessageHandler

  setBackgroundMessageHandler -.-> app
  setBackgroundMessageHandler -.notification 띄우는 주체가 아직 불확실.-> notification
  onMessage --> app

  app --show--> notification
  notification --press--> onForegroundEvent
  notification -.press.-> onBackgroundEvent

  onForegroundEvent --> app2[app]
  onBackgroundEvent --> app2[app]

  app2 -.-> getInitialNotification -.background 에서 받은 경우.-> app2 
  app2 --deeplink--> Linking.openURL
  app2 ==fast track 인가?\nnavigate==> deeplink-url

  Linking.openURL --> Linking.addEventListener`url`
  Linking.openURL -.-> app3[app]

  Linking.addEventListener`url` --> app3[app]
  app3 -.-> Linking.getInitialURL -.background 에서 받은 경우.-> app3[app]

  app3 --navigate---> deeplink-url

  style notification fill:#b3d9ff
  style onForegroundEvent fill:#bbf
  style onBackgroundEvent fill:#bbf
  style getInitialNotification fill:#bbf

  style onMessage fill:#f66
  style setBackgroundMessageHandler fill:#f66

  style Linking.openURL fill:#e91
  style Linking.addEventListener`url` fill:#e91
  style Linking.getInitialURL fill:#e91
```
- 흐름도를 보면 Linking.openURL 을 타지 않고 바로 네비게이션 처리가 가능하다
- `Linking` 을 사용하는 이유
  - `notification` 을 통하지 않고 다른 앱이나 웹에서 [[deeplink]] 를 받기 위해서라도 필요할 것으로 생각된다
- 헷갈릴까봐 그래프에 표시 하지 않았지만 `@react-native-firebase/messaging` 에서 제공하는 이벤트 핸들러는 `@notifee/react-native` 의 on{Fore/Back}groundEvent 에도 동일하게 전달되는 것을로 보인다.  `DELIVERED` 타입 좀 더 확인을 용의가 있다면 firebase 이벤트 핸들러를 제거할 수 있을 수 있다
### fcm 메시지 수신
```mermaid
flowchart TD
  notifee(#notifee/react-native 사용시 해당 패키지 함수 사용)
  style notifee fill:#bbf,stroke:#f66,stroke-width:2px,color:#fff,stroke-dasharray: 5 5

  getInitialNotification(getInitialNotification)
  style getInitialNotification fill:#bbf,stroke:#f66,stroke-width:2px,color:#fff,stroke-dasharray: 5 5

  fcm-payload -.-> background
  fcm-payload -.-> quit
  background --> .setBackgroundMessageHandler
  quit --> .setBackgroundMessageHandler

  fcm ==send==> fcm-payload
  fcm ==new token==> .onTokenRefresh
  fcm-payload --> foreground
  foreground --> .onMessage

  .setBackgroundMessageHandler -.- app
  .onMessage -.- app
  .onTokenRefresh -.- app
  
  app -.to foreground.-> getInitialNotification
```

### 노티피케이션을 보여주는 과정
```mermaid
flowchart LR
  app -.- .showNotification
  
  .showNotification ==========> notification
```

### 노티피케이션 데이터에 따른 핸들링(e.g. deeplink)
```mermaid
flowchart TD
  notifee(#notifee/react-native 사용시 해당 패키지 함수 사용)
  style notifee fill:#bbf,stroke:#f66,stroke-width:2px,color:#fff,stroke-dasharray: 5 5

  .onNotificationOpenedApp(.onNotificationOpenedApp)
  style .onNotificationOpenedApp fill:#bbf,stroke:#f66,stroke-width:2px,color:#fff,stroke-dasharray: 5 5

  %% style .onNotificationOpenedApp fill:#f9f,stroke:#333,stroke-width:4px

  notification --click--> .onNotificationOpenedApp
  .onNotificationOpenedApp -.deeplink.- Linking.openURL`deeplink`

  Linking.openURL`deeplink` -.trigger event.- event`url`
  event`url` -.-> foreground
  event`url` -.-> background
  foreground --> Linking.addEventListener`url`
  background --> Linking.getInitialURL

  Linking.addEventListener`url` -.-> app
  Linking.getInitialURL -.-> app
```

## link
- [[fcm]]
