# service

[[kubernetes]] service
label selector 를 통해서 load balancing 을 하며 endpoint 가 자동으로 붙는다
endpoint 가 생성되지 않으며 이를 위해서는 label selector 를 스펙 명시하지 않으면 된다
## headless service
headless service 는 `sepc.clusterIP: None` 을 통해서 만들어지며 이 경우에 kube-proxy 에서 처리하지 않게된다.
도메인 네임을 통해 접근이 가능하다
### endpoint 가 있는 service
+ https://interp.blog/k8s-headless-service-why/
+ https://dev.to/eddiehale3/building-a-headless-service-in-kubernetes-3bk8
- [[nslookup]] 을 사용하면 일반적인 service 경우 해당 ip 를 노출한다
- headless service 의 경우에는 endpoint 목록 전체를 **랜덤**한 순서로 제공한다(로드밸런싱이 없음)
  - curl 등은 이 중 첫번째 endpoint 와 통신한다
  - 아마도 하나의 클라이언트에서는 하나의 pod 와 연결이 이루어져야한다고 의미가 있다고 생각한다(stateful set, sticky session)
    - [ ] 실제로 그런지는 확인이 필요, 그럼 envoy 같은게 왜있어;
### endpoint 가 없는 service
1. selector 를 지정하지 않으면 endpoint 가 생성되지 않는다
2. service 명과 같은 endpoint 를 생성한다
headless service 는 이미 endpoint 가 있어서 접근이 가능한 클러스터 외 서비스를 붙일 때 사용가능하다.

공식 문서에 설명이 잘 되어 있으니 이를 참조한다.
+ https://kubernetes.io/ko/docs/concepts/services-networking/service/
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
---
apiVersion: v1
kind: Endpoints
metadata:
  # 여기서의 이름은 서비스의 이름과 일치해야 한다.
  name: my-service
subsets:
  - addresses:
      - ip: 192.0.2.42
    ports:
      - port: 9376
```

service 에는 ping 을 할 수 없다. [[curl]], wget 등으로 접근을 확인한다

## link
- [[kubernetes]]
