# google

## sites
+ https://console.cloud.google.com
+ https://console.firebase.google.com
+ https://play.google.com/console
+ https://search.google.com/search-console
+ https://www.google.com/adsense

## 플레이 콘솔
+ https://play.google.com/console

### 개발자 등록
+ [[diary:2023-12-25]] 완료
+ https://play.google.com/apps/publish/
1. 계정 유형:  개인
2. 내정보:  대충
3. 개발자 계정
  - 개발자 계정 이름: 아무거나
  - 담당자 이름: 아무거나
  - 연락처 이메일 주소
  - 선호 언어
  - 연락처 주소
  - 연락처 전화번호
4. 앱: 설문조사류
5. 결재
6. 확인
  - [ ] 계정확인
    - 특정 기간을 지정하거나 지정되서 진행되며 확인되지 않으면 **계정 삭제**됨
    - 공문서가 하나 있는듯?
  - [X] 본인인증

### 앱등록
- 플레이 콘솔에서 앱 생성

## 인증

### OAuthClient
- android
  - 생성에 필요한 *SHA-1 인증서 디지털 지문* 은 [[keytool]] 참고
  - 패키지 이름은 [[react-native#DEVELOPER_ERROR]] 참고
- ios
- web

### error
#### 403 오류, access_denied
- 아직 제출 되지 않은 앱의 경우 test 사용자를 등록해야한다
  + https://console.cloud.google.com/apis/credentials/consent
    - 테스트 사용자 -> *ADD USERS* -> 로그인하려는 이메일 추가
    - 이후에 신뢰관련된 화면이 몇개 나오는데 *계속*  눌러주면된다.

## link
- [[exiftool]]
- [[synology]]
- [[google-workspace]]
- [[apple]]
