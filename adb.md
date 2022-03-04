## wifi 디버깅

케이블로 폰 연결
```sh
adb tcpip 5555
```

케이블 제거
설정 -> 연결 -> WIFI -> 더보기 -> IP 확인
```sh
adb connect [IP] # 연결
adb devices # 확인
adb shell dumpsys activity activities > activity_dump.txt
```

## -
[[react-native]] [[metro]] 연결시에 필요, 재부팅마다 해줘야하는듯
```sh
adb [-s DEVICE_ID] reverse tcp:8081 tcp:8081
```

## related
- [[android-studio]]
- [[emulator]]
