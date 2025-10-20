# tizen
- 삼성tv os로 사용됨

## package 설치
- tizen [[tv]] apps 에서 developer mode on
  + https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html
  - 요약 tv on -> apps -> 12345 입력 -> ip는 접속할 클라이언트 ip(eg. notebook ip) -> 2초간 파워 눌러서 전원 껏다가 켬
- 접속할 클라이언트에 studio 설치, cli 건 gui건 설치하면 home 에 폴더가 생길 것, 난 cli로 진행
  - downloadl -> chmod +x -> 실행
  + https://developer.tizen.org/development/tizen-studio/download/
- 설치하려는 `wgt` 파일 download
- 설치
  ```sh
  ~/tizen-studio/tools/sdb connect [TV_IP]
  ~/tizen-studio/tools/sdb devices # 목록에서 TV_DEVICE_NAME 확인
  ~/tizen-studio/tools/ide/bin/tizen install -n [WGT_FILEPATH] -t [TV_DEVICE_NAME]
  ```

## link
- [[moonlight]]
- [[jellyfin]]
