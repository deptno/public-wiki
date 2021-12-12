## wifi 디버깅

케이블로 폰 연결
```sh
`adb tcpip 5555
```
케이블 제거
설정 -> 연결 -> WIFI -> 더보기 -> IP 확인
```sh
adb connect [IP] # 연결
adb devices # 확인
```

## -
```sh
adb reverse tcp:8081 tcp:8081
```
