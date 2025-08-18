# cloudflare

## 인증서
- 무료 플랜 기준으로 2단계 서브도메인은 지원하지 않음, `a.b.exmaple.com` 지원x

## 도메인 이전 -> cloudflare
### dns record 이전
- cloudflare 에서 `connect a domain` 후 기존 호스트를 하고 있는 곳에서 일단 dns record 를 모두 옮기고 cloudflare 에서 제공하는 ns 를 기존 서비스하는 곳에서의 ns 를 지우고 대체함
  - `route53` 의 `Hosted zones` 에서 수정하는 것이 아니라
  - 도메인을 등록지를 기준으로 name server 를 변경해야함
  - [[aws]] 인 경우 `Registered domains` 에서 ns를 바꿔야함 `Hosted zones` 이 아님
- `pending` 시간이 필요하므로 `pending` 상태일 수 있음
- 애후 `transfer domain` 진행하며 이때 기존 서비스하던 곳에서 `auth key` 와 `transfer lock` 설정등은 **off** 처리되어야함

### domain transfer
- 도메인 아예 이전은 `dns record` 이전 이후에 가능
- 완전히 이전 되서 cloudflare 를 사용하고 있는 상태
- transfer lock 해제 후 auth code 발급 받아서 이전 신청 후, cloudflare -> `manage registration` -> `manage domains` 에서 진행
  - 수일 소모 + 이전시에 1년 이상 갱신 필요, 이건 cloudflare 랑 무관하게 이전시 필요한 조건

## 에러처리 :error:
### proxy
- [[harbor]] 에 push 하는 속도가 현저하게 느려짐, 가끔 에러도 발생
```
Post "https://domain.com/v2/xxxxx/xxxx/blobs/uploads/": proxyconnect tcp: dial tcp 192.xxx.xx.1:xxxx: i/o timeout
```

## link
- [[aws]]
- [[domain]]
