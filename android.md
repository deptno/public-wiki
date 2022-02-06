# android

## Android android-studio
- Actions -> New Module

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


## Resource|리소스
### 구성 한정자
- https://developer.android.com/guide/topics/resources/providing-resources?hl=ko
해당 리스트를 통해 리소스 디렉토리에 suffix 를 붙임으로써 특정 상황에서 사용되는 리소스 임을 명시한다. 언어, 디바이스 방향 상태, 해상도 등이 포함된다. 중복 명시도 가능한데 순서가 존재한다.
- res/layout/activity_main.xml - 세로 모드에서 사용
- res/layout/`land`/activity_main.xml - 가로 모드에서 사용

## related
- [[flipper]]
- [[scrcpy]]
- [[adb]]
- [[android-studio]]
- [[react-native]]
- [[emulator]]
- [[java]]
- [[kotlin]]
