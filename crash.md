# crash
- [[app]] 이 죽는상황

## debugging
### react-native
- 디바이스와 연결 후 [[flipper]] 연결하면 디바이스 로그 확인가능
  - debug 빌드

## case
- [[android]] 이 시작하자 마자 죽는다
  - 디버깅해보니 아래와 같은 정보
    ```sh 
    FATAL EXCEPTION: mqt_native_modules
    Process: xx.xxxxx.xxx, PID: 14673
    com.facebook.react.common.JavascriptException: Error: RNGoogleSignin: offline use requires server web ClientID, js engine: hermes, stack:
    configure@1:741494
    anonymous@1:605162
    loadModuleImplementation@1:24012
    guardedLoadModule@1:23554
    metroRequire@1:23176
    anonymous@1:604689
    loadModuleImplementation@1:24012
    guardedLoadModule@1:23554
    metroRequire@1:23176
    anonymous@1:30212
    loadModuleImplementation@1:24012
    guardedLoadModule@1:23511
    metroRequire@1:23176
    global@1:22728
    ```

## link
- [[app]]
- [[react-native]]
- [[anr]]
