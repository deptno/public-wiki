# adb|Android Debug Bridge
+ https://adbshell.com

## wifi 디버깅
- 케이블로 폰 연결
  ```sh
  adb tcpip 5555
  ```
- 케이블 제거
- 설정 -> 연결 -> WIFI -> 더보기 -> IP 확인
  ```sh
  adb connect [IP] # 연결
  adb devices # 확인
  ``` 

## command
+ https://adbshell.com/commands
- reverse - 기기의 localhost 접근 포트를 연결된 pc 의 포트로 전달
  - [[react-native]] [[metro]] 연결시에 필요, 재부팅마다 해줘야하는듯
  ```sh
  adb [-s DEVICE_ID] reverse tcp:8081 tcp:8081
  adb [-s DEVICE_ID] reverse --list # 목록
  ```
- 덤프
  ```sh
  adb shell dumpsys activity activities > activity_dump.txt
  ```
- 덤프
  ```sh
  adb push app/build/outputs/apk/release/app-release.apk /storage/self/primary/Download/ # 디바이스로 파일 복사
  adb pull /storage/self/primary/app-release.apk  # 디바이스로 부터 파일 복사
  ```

## error
### error: device unauthorized.
```sh
adb: error: device unauthorized.
This adb server's $ADB_VENDOR_KEYS is not set
Try 'adb kill-server' if that seems wrong.
Otherwise check for a confirmation dialog on your device.
```

### error: more than one device/emulator
- `adb devices` 보면 2개 이상의 디바이스가 확인된다
- `adb -s [DEVICE_ID] shell` 을 통해 device shell 로 진입해서 명령어를 처리한다
- `adb -s [DEVICE_ID] [COMMAND] [COMMAND_OPTION]` 을 통해 명령어를 특정 디바이스에 특정한다

## link
- [[android-studio]]
- [[emulator]]
- [[android]]
- [[react-native]]
