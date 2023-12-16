# fcm
firebase cloud message

## overview
+ https://seungwoolog.tistory.com/88 참조

## 사용패턴
- notification
- data sync
  - [ ] 활용패턴이 있을 것 같음 특정 ui 버전을 보게 한다던가, de-feature 해버린다던가
- updating ui

## 사용
- 프로젝트생성
  + https://console.firebase.google.com/u/0/?hl=ko
- 프로젝트화면에서 *앱에 Firebase를 추가하여 시작하기* 를 통해 [[iOS]], [[android]] 관련 설정
- [[android]] 앱에 Firebase 추가
  - 패키지 네임 [[react-native]] 프로젝트 기준으로 `android/app/build.gradle` 확인
  - 및 디버그 서명 인증서 SHA-1 은 [[keytool]] 을 통해 정보 확인
  - 확인 된 정보로 앱등록 -> google-service.json 다운로드 -> android/app 폴더에 추가
- client
  + https://rnfirebase.io/messaging/usage
- server
  + https://rnfirebase.io/messaging/server-integration

### client
- [[ios]] 는 백그라운드에서 메시지를 받았을때 리액트 루트가 마운트되며 이로인한 문제를 방지하기 위한 설정이 필요하다
  + https://rnfirebase.io/messaging/usage#background-application-state

#### firebase.json
> 추가 설정을 위해서 자동으로 해주는 것들을 `disabled` 시키는 등, 특정 설정 가능
- [[ios]]
  - disable APNs auto registration
  - foreground presentation options - notification 보여지는 방식
- [[android]]
  - background handler timeout 설정, background, quit 상태에서 메시지를 핸들링할 타입지정 ->  지나면 앱 quit으로 추정
  - Notification Channel ID
    - [ ] 학습 필요 [[@todo]]
  - Notification Color
- Auto initialization

#### app 의 상태따른 행동
- 상태
  - foreground
    - `messaging().onMessage()`
  - background
    - `messaging().setBackgroundMessageHandler()`
  - quit - 종료된 경우
- 상태에 따라 다르게 행동하게되며 이해가 중요하다고 함 읽어볼 것
  + https://firebase.google.com/docs/cloud-messaging/concept-options?hl=ko

### 메시지 타입
> data only 메시지는 low priority 로 간주되며 앱을 깨우지 않는다,  우선순위를 강제하면 깨울 수 있다
- data + notification
- data

## 언급되는 라이브러리
- https://github.com/invertase/notifee
  - android notification ux 까지 변경지원하는 것으로 언급됨
  - 이전에 사용되던 https://github.com/zo0r/react-native-push-notification 는 버전 업데이트 멈춘듯 2021-10-01




## link
- [[firebase]]
- [[iOS]]
- [[android]]
- [[react-native]]
