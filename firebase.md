# firebase
- firebase 프로젝트를 등록하면 google cloud console 에 firebase 가 테스트 사용 및 API 키를 자동으로 등록한다
- firebase 프로젝트를 등록하면 google cloud console 에 firebase 서비스계정을 자동으로 등록한다

## client
- console.firebase.google.com 에서 각 플랫폼별 `google-services.json` 을 다운받는다
- 문서가 두개가 있는데 firebase 문서가 아닌 rnfirebase.io *Getting Started* 문서를 따라간다
  - [ ] https://firebase.google.com/docs/android/setup?authuser=0&hl=ko#groovy
  - [X] https://rnfirebase.io
- [[android]]
  - `android/app` 에 `google-services.json` 복사
  - android/build.grade dependecies 에 classpath 'com.google.com:google-service:4.3.15' 추가한다
    - google sigin 관련 라이브러 설치하면서 이미 설정되어있음
  - android/app/build.grade 에서 apply plugin
- [[ios]]
  - console.firebase.google.com 에서 ios 앱도 프로젝트 설정을 해주어야한다
  - 개발자 등록후 진행

## serverside
+ https://firebase.google.com/docs/cloud-messaging/server?hl=ko
- console.firebase.google.com 에서 프로젝트 ->  프로젝트 설정 ->  서비스 계정 -> *새 비공개 키 생성*
- 

## [[error]]
### Error: No Firebase App '[DEFAULT]' has been created - call firebase.initializeApp()
```sh 
Error: No Firebase App '[DEFAULT]' has been created - call firebase.initializeApp()
```
- [[android]] 에서 발생했으며 설정이슈 [[#client]] 설정 참고

### TypeError: Cannot read properties of undefined (reading 'INTERNAL')
```sh 
TypeError: Cannot read properties of undefined (reading 'INTERNAL')
    at initializeApp (.yarn/cache/firebase-admin-npm-12.0.0-a20a06d34c-70e619250d.zip/node_modules/firebase-admin/lib/app/firebase-namespace.js:246:21)
```
+ https://firebase.google.com/docs/admin/setup?hl=ko#add-sdk
- 위 **공식**  문서를 그대로 따라하면 에러가 발생한다 [[diary:2023-12-20]]
- 아래 링크를 참조하면 모듈이 클래스로 생성되어서 `this` 에러가 나는 것이라고 말하고 있다
  + https://stackoverflow.com/questions/66260225/cannot-export-firebase-admins-initializeapp-function
- 실제로 된다 `{ initializeApp }` -> `admin` 으로 모듈을 로드해서 사용
- as-is
```ts 
import { initializeApp, credential } from 'firebase-admin'
import { applicationDefault } from 'firebase-admin/app'

const app = initializeApp({
  credential: applicationDefault()
})
```
- to-be
```ts 
import admin from 'firebase-admin'
import { applicationDefault } from 'firebase-admin/app'

const app = admin.initializeApp({
  credential: applicationDefault()
})
```

### The default Firebase app already exists. This means you called initializeApp() more than once without providing an app name as the second argument. In most cases you only need to call initializeApp() once. But if you do want to initialize multiple apps, pass a second argument to initializeApp() to give each app a unique name.
```sh 
  errorInfo: {
    code: 'app/duplicate-app',
    message: 'The default Firebase app already exists. This means you called initializeApp() more than once without providing an app name as the second argument. In most cases you only need to call initializeApp() once. But if you do want to initialize multiple apps, pass a second argument to initializeApp() to give each app a unique name.'
  },
  codePrefix: 'app',
  page: '/api/test/fcm'
```
- [[nextjs]] hmr 과 함께 모듈이 두번로딩되어서 발생하는 것으로 추정
  + https://github.com/firebase/firebase-admin-node/issues/2111#issuecomment-1636441596
- 최종적으로 다음과 같은 모습이 되어야함 - [[diary:2023-12-20]]
```ts 
import admin from 'firebase-admin'
import { applicationDefault, getApp, getApps } from 'firebase-admin/app'

const apps = getApps()
const app = apps[0] ?? admin.initializeApp({
  credential: applicationDefault()
})
```

## link
- [[fcm]]
