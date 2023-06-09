# loki

metadata 기반으로 쿼리가 가능한 데이터베이스

## installation
```sh
helm pull grafana/loki --untar
# vim loki/values.yaml
helm upgrade loki ./loki --install --create-namespace -n loki
```

- kubernetes single cluster 인 경우, self monitoring 을 off 시켜서 `loki-logs-*` 파드를 제거함
- loki-canary 도 상태 확인으로 보이는데 제거하는것이 나을 수 있으나 일단 기본 값 유지

## error
`loki-logs-[XXXX]` pod가 계속 `CrashLoopBackOff` 에러가 발생한다. single instance 라 발생하는 것일지도..

- version
  - Chart version: 4.0.0
  - Loki version: 2.7.0
- container log
  - grafana-agent
```sh
2023-01-23 15:27:28.144680 I | error loading config file /var/lib/grafana-agent/config/agent.yml: error reading config file open /var/lib/grafan
a-agent/config/agent.yml: no such file or directory
```
  - config-reloader
```sh
level=info ts=2023-01-23T15:28:14.093745662Z caller=main.go:147 msg="Starting prometheus-config-reloader" version="(version=0.47.0, branch=refs/
tags/pkg/client/v0.47.0, revision=539108b043e9ecc53c4e044083651e2ebfbd3492)"
level=info ts=2023-01-23T15:28:14.093776252Z caller=main.go:148 build_context="(go=go1.16.3, user=simonpasquier, date=20210413-15:46:43)"
add config file /var/lib/grafana-agent/config-in/agent.yml to watcher: create watcher: too many open files
```
- 해결
  - [ ] TODO: 고친다
    - [[error##too many open files]] 참고
  - [X] self monitoring 을 제거한다. `values.yaml`
```yaml
test:
  enabled: false
selfMonitoring:
    enabled: false
serviceMonitor:
    enabled: false
```
---
```sh
level=warn ts=2023-01-23T19:45:30.361792194Z caller=logging.go:86 traceID=1c106c8039718249 orgID=fake msg="POST /loki/api/v1/push (500) 375.897µ
s Response: \"at least 2 live replicas required, could only find 1\\n\" ws: false; Content-Length: 6360; Content-Type: application/x-protobuf; U
ser-Agent: promtail/2.7.1; "
```

## link
- [[kubernetes]]
- [[prometheus]]
- [[grafana]]
- [[promtail]]
- [[elastic-search]]
