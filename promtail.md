# promtail

log harvest (수집) 로 생각됨

filebit 이랑 비슷한건가 아직 elk, efk 쓴지가 오래되서 기억이 안남, 책에서는 fluentd 를 언급

## error
```su
level=error ts=2023-01-23T19:04:06.750841317Z caller=client.go:390 component=client host=loki-headless.loki:3100 msg="final error sending batch"
 status=401 error="server returned HTTP status 401 Unauthorized (401): no org id"
```
[[loki]] `auth_enabled: false` 설정으로 disabled 처리

## related
- [[loki]]
- [[fluentd]]
- [[logstash]]
