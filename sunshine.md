# sunshine
- [[moonlight]] 의 host 프로그램
- 초고화질, 저지연 스트리밍으로 remote 게임 플레이를 가능하게한다

## virtual display driver 설치
- 해당 드라이버 설치를 통해 moonlight 를 통해 접속시에 가상의 디스플레이를 통해 클라이언트에 맞춤형 디스플레이 지원이 가능
- 호스트의 디스플레이로부터 종속을 피한다
- 드라이버 인스톨 후 설정
  - display driver 설치
  - 지원하고자 하는 옵션 선택(10bit 등)
  - [[windows]] 디스플레이 설정에서 디스플레이 크기 설정
  - sunshine 설정에서`audio/video` -> `장치 ID 표시` 에 입력
    - device id 를 입력한다 `{xxxxx-xxx-xx-xx-xxx}` 형태의 형식
    - 드라이버 설치 후 장치관리자에서 속성에서 확인 가능하다
    > **드라이버 프로그램종료 후 id 가 바뀌는 것 같기도 **
  - [[moonlight]] 를 통한 접속

## link
- [[moonlight]]
- [[windows]]
