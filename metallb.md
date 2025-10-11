# metallb

load balancer for [[on-premise]]

2023-01-08 
v0.13.7

## 설정
+ [[diary:2025-10-11]]
- 싱글 노드 클러스터인 경우 `ignoreExcludeLB: true` 설정 필요
  - 기본적으로 control plane 인 경우 load balancer 생성하지 않도록 레이블이 되어있음
  - 레이블을 제거하던가 해당 옵션 설정하던가 둘중 하나로 처리가능

## link
- [[kubernetes]]
- [[kube-vip]]
