# fcm
firebase cloud message

## overview
+ https://seungwoolog.tistory.com/88 참조

## 사용
- 프로젝트생성
  + https://console.firebase.google.com/u/0/?hl=ko
- 프로젝트화면에서 *앱에 Firebase를 추가하여 시작하기* 를 통해 [[iOS]], [[android]] 관련 설정
- [[android]] 앱에 Firebase 추가
  - 패키지 네임 [[react-native]] 프로젝트 기준으로 `android/app/build.gradle` 확인
  - 및 디버그 서명 인증서 SHA-1 은 [[keytool]] 을 통해 정보 확인
  - 확인 된 정보로 앱등록 -> google-service.json 다운로드 -> android/app 폴더에 추가
- Cloud Message 진입 + [[fcm]]


## link
- [[firebase]]
- [[iOS]]
- [[android]]
- [[react-native]]
