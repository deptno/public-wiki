# proxyman

- 구매일: [[diary:2024-03-04]]
  - $79 + VAT 10% = 󰞽118,292
+ proxyman.io

실기기 패킷 캡쳐가 가능

## 설치
```sh
brew install --cask proxyman
```

1. 실행 후 메뉴에서
- Install Certificate on Mac
- Install Certificate on iOS
- Install Certificate on Android
2. 설정에서 설치
3. 프록시맨에서 `Enable only this domain` 설정을 해줘야지 됨


## 사용
### [[ios]]
- 앱 설치 후 실행 후 instruction 을 따르면 됨
  - 인증서 설치
  - 인증서 신뢰
  - vpn 활성화

### mac -> [[ios]]
#### mac
- mac 에서 실행
- *Install Certificate on iOS* 메뉴의 인스트럭션을 따름
  - 인증서 설치
    - **주의** [[ios]]
      - vpn **비** 활성화 상태에서 인증서 설치, 아마도 `http://proxy.man/ssl`
      - 인증서 이름에 mac 의 이름이 떠야함
  - 인증서 신뢰
    - 신뢰를 하지 않는 경우 `Internet Error 999` 발생
- [[ios]] 사용시에서도 vpn off,

## [[error]]
### Internet Error 999
- mac 에서 [[iphone]] 에 있는 데이터를 보려고할때 발생
- mac 의 인증서가 [[iphone]] 에 설치되어야 한다
  - wifi proxy 시 설정을 확인
  - vpn 꺼짐 확인
- [[iphone]] 에 설치 후 활성화 해줘야한다
- [[ios]] 에서 vpn 은 꺼둔다
- mac 을 리붓하기도 함

## link
- [[ios]]
- [[android]]
- [[dev-tools]]
