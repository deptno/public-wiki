# cloudflare

## 도메인 이전 -> cloudflare
- 두단계
- cloudflare 에서 `connect a domain` 후 기존 호스트를 하고 있는 곳에서 일단 dns record 를 모두 옮기고 cloudflare 에서 제공하는 ns 를 기존 서비스하는 곳에서의 ns 를 지우고 대체함
- `pending` 시간이 필요하므로 `pending` 상태일 수 있음
- 애후 `transfer domain` 진행하며 이때 기존 서비스하던 곳에서 `auth key` 와 `transfer lock` 설정등은 **off** 처리되어야함

### from [[aws]]
- route53 -> registered domains
  - 여기에 없으면 다른 곳에서 등록하고 hosted zone 만 쓰는 경우니 도메인 구입처 등 다른 곳 찾아볼 것

## link
- [[aws]]
- [[domain]]
