# home-assistant

## 설치 기록
- kubernetes resource 생성
  - pv
  - pvc
  - dp
  - svc
  - ingressroute
- configuration.yaml 설정
  - http - reverse proxy
  - homeassistant - external_url(webhook url 표시때문)
- 설정 -> 기기설정 -> 기기추가(smartthings) -> 외부링크: 토큰 발급
- 설정 -> 기기설정 -> smartthings -> 웹훅 url 확인 -> 발급받은 토큰 입력
- google-assistant 연결
  + https://www.home-assistant.io/integrations/google_assistant/#yaml-configuration
  - 최종적으로 재시작 까지 후에 `google home` 앱에서 기기연결을 통해 ha 와 연동
  - 스마트 싱스는 ha 밑에 붙어있으므로 `google home` 에 연동된 경우 해제해준다.
  - [X] 음성인식 테스트
- homekit bridge 연결
  + https://www.home-assistant.io/integrations/homekit/
  - 해보니가 마지막에 iOS 에서 연결할때 에러
    - [ ] 설정에 포트가 있는거 보니 포트 노출이 안되서 그런것으로 보이는데 svc 생성 테스트 필요
## configuration
### reverse proxy
외부 접근 허용을 위해서는 리버스 프록시 설정이 필요하다.

```sh 
- (MainThread) [homeassistant.components.http.forwarded] A request from a reverse proxy was received from 10.244.182.150, but your HTTP integration is not set-up for reverse proxies
```
`configuration.yaml` 파일을 수정한다
```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
  - 10.244.182.0/24
  ip_ban_enabled: true
  login_attempts_threshold: 5
```
+ https://community.home-assistant.io/t/a-request-from-a-reverse-proxy-was-received-from/314089/4
+ https://11q.kr/www/bbs/board.php?bo_table=co3&wr_id=670
한줄한줄 확인을 하지 않아 확실히는 알 수 없지만 pod 로그에 보면 찍히는 요청 ip 를 trusted_proxies 에 추가하면 된다.
위에 두줄 옵션이 관련 설정일 것으로 보임
설정후에는 UI 에서 재시작을 해준다.
### external_url
버그인지 [[diary/2023-07-11|2023-07-11]] 현지 기준으로 설정 -> 시스템 -> 네트워크 설정으로 이동하면 화면에 아무것도 노출되지 않았다.
또 다시 ssh 로 접속해서 `configuration.yaml` 을 열고 아래 내용을 추가한다
```yaml
homeassistant:
  external_url: "https://www.example.com"`
```

## [[error]]

### 403 forbidden
> 로그인화면자체가 안뜨고 [[403]] text 가 뜨는 경우
```sh 
2023-10-01 11:55:51.173 ERROR (MainThread) [homeassistant.components.google_assistant.http] Request for https://homegraph.googleapis.com/
v1/devices:requestSync failed: 500
2023-10-01 11:56:45.968 ERROR (MainThread) [homeassistant.components.google_assistant.http] Request for https://homegraph.googleapis.com/
v1/devices:reportStateAndNotification failed: 404
```
+ https://developers.google.com/assistant/ca-sunset?hl=ko
  - 마침 에러가 있어서 혼동이 있었지만 이건 아닌 것으로 보임

```ip_bans.yaml
192.168.0.xxx:
  banned_at: '2023-09-30T04:33:49.311517+00:00'
```
`ip_bans.yaml` 해당 내용을 삭제하던지 해당 파일을 삭제하면 된다.
- [[@todo]] ha 가 컨테이너로 떠있는 호스트의 ip가 밴이되서 내부접근이 모두 밴 된 것으로 보인다
- [[diary:2023-11-12]]

### Username already exists
> 삭제된 유저를 재 생성하려고 하려고 하면 에러가 난다
- [[pv]] 로 접근해서 데이터를 모두 삭제한다
```sh
cat person # user_id 확인
rm frontend.user_data_[user_id] # user 제거
vi auth_provider.homeassistant # 해당 user 정보 제거
grep -r [user_id] . # 리스팅된 파일에서 해당 정보가 포함된 데이터 제거
```
- 재부팅(컨테이너 재실행) 후 생성한다

## links
- [[kubernetes]]
- [[smartthings]]
