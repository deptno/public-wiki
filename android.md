# android

## 개발

### 개발자 콘솔
+ https://play.google.com/console

### 물리 디바이스 연결
- 실기기 개발자모드 활성화
  - 설정 ->  시스템 ->  휴대폰 정보 ->  소프트웨어 정보 -> 빌드 번호 7회 터치
- USB 디버깅 활성화
  - 설정 ->  시스템 ->  개발자 옵션 -> USB 디버깅 활성화 -> 추가 다이얼로그 확인
- `adb devices` 로 물리 디바이스가 연결된 것 확인 가능

### SHA-1 인증서 디지털 지문
- [[keytool]] 참고
- google 인증에서 사용됨

## Android android-studio
- Actions -> New Module

### [[mac]] -> [[android]] 파일복사
#### android file transfer 를 통한 복사
```sh 
brew install --cask android-file-transfer
```
- 설치하고 나서 안드로이드 기기를 연결하면, finder 형태로 디바이스 스토리지가 보인다

#### [[adb]] 를 이용한 복사
```sh 
adb push app/build/outputs/apk/release/app-release.apk /storage/self/primary/Download/
```

## 설정
### Android Studio
#### Auto import
- Settings -> Editor -> General -> Auto import -> Kotlin
  - [X] Add unambiguous imports on to fly
  - [X] Optimize imports on the fly (for current project)
  
### Emulator|에뮬레이터
- 아래서 위로 스와이프 -> 시스템
  - Auto Rotate
  - System Language -> 테스트할 언어 첫번째로

## Layout|레이아웃
- LinearLayout
- RelativeLayout
- FrameLayout
- GridLayout
- ConstraintLayout

## viewBinding
- as-is
```kt
overridefunonCreate(savedlnstanceState: Bundle?) {
    super.onCreate(savedlnstanceState)
    
    setContentView(R.layout.main_activity)
    findViewById(R.id.text1)
}
```
- to-be
findViewById 사용을 피하기 위해서 [[gradle]] 에서 `viewBinding` 설정이 사용된다.
```gradle
android {
    viewBinding {
        enabled true
    }
}
```
```kt
overridefunonCreate(savedlnstanceState: Bundle?) {
    super.onCreate(savedlnstanceState)
    
    val binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
}
```

## Event|이벤트
- Single Abstract Method(SAM) - 오브젝트 대신 메소드를 할당하여 처리한다.

```kt
override fun onCreate(savedlnstanceState: Bundle?) {
    super.onCreate(savedlnstanceState)
    
    val binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
    
    binding.startButton.setOnClickListener {
        // 이벤트 처리
    }
}

```

## Life Cycle|생명주기
- 액티비티 시작
- onCreate call setContentView
- onStart
- onResume
- 액티비티 실행(setContentView 에서 지정한 UI)
- onPause 포커스가 없는상태, 화면 분할 등 -> onResume
- onStop 비활성화 상태, 홈 버튼을 눌러서 나간 상태 등-> onRestart -> onStart
- onDestroy -> onCreate
- 액티비티 종료
[[@todo]] restart 등 추가 필요

### 화면을 회전화는 경우
onResume -> onPause -> onStop -> onSaveInstanceState -> onDestroy
-> onCreate -> onStart -> onRestoreInstanceState -> onResume

## Intent|인텐트
인텐트를 통해 시스템에 액티비티의 생성을 요청한다.
```kotlin
val intent = Intent(this, MainActivity::class.java)
intent.putExtra("key", "value")
startActivity(intent)
startActivityForResult(intent, intrequestCode)
```
생성 후 받는 쪽에서는 `getIntent` 를 통해서 인텐트를 얻고 인자를 가져올 수 있다.
인텐트 실행시에 특정 앱을 지정하고싶다면 패키지를 설정한다.
```kotlin
intent.setPackage("com.google.android.apps.maps")
```

### error
#### 없는 activity 를 지정한 경우(없는 intent)
실행한 쪽에서 에러가 난다.
#### Coludn't sign in
```text
There was a problem communicating with Google servers.
```

### 액티비티 실행
#### 명시적 
앱의 내부에서는 명시적으로 클래스를 지정해서 실행, 암시적으로도 가능하나 권장된다.
암시적으로는 실행할 수 없는 경우도 존재한다.
```kotlin
val intent = Intent(this, MainActivity::class.java)
```

#### 암시적
외부에서 액티비티를 실행할 수 있도록 manifest 파일에 intent 설정을 해줘야한다.
```xml
<activity android:name=".oneActivity" />
<activity android:name=".TwoActivity">
  <intent-filter>
    <actlon android:name="ACTION EDIT" />
  </intent-filter>
</activity>
<service ... />
<receiver ... />
```

### 인텐트 필터
데이터 전달
- <action /> - android:name 은 유일하지 않아도된다. 기능을 보여주는 이름을 사용하자. eg, android.intent.action.MAIN
- <category /> - android:name -> android.intent.category.LAUNCHER
- <data /> - 필요한 데이

> 런처앱은 `android.intent.category.LAUNCHER` 카테고리의 `action.intent.action.MAIN` 을 가져와서 리스트업하는 액티비티다.



## Resource|리소스
### 구성 한정자
- https://developer.android.com/guide/topics/resources/providing-resources?hl=ko
해당 리스트를 통해 리소스 디렉토리에 suffix 를 붙임으로써 특정 상황에서 사용되는 리소스 임을 명시한다. 언어, 디바이스 방향 상태, 해상도 등이 포함된다. 중복 명시도 가능한데 순서가 존재한다.
- res/layout/activity_main.xml - 세로 모드에서 사용
- res/layout/`land`/activity_main.xml - 가로 모드에서 사용

## [[error]]

###  설치 실패
- [[firebase]] app distribution, [[app]] tester 등을 통해서 앱을 설치하려고하는데 계속 에러가 난다면 빌드를 universal apk 로 진행한다 [[diary:2023-12-31]]

## link
- [[flipper]]
- [[scrcpy]]
- [[adb]]
- [[android-studio]]
- [[react-native]]
- [[emulator]]
- [[java]]
- [[kotlin]]
- [[proxyman]]
- [[mac]]
