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
