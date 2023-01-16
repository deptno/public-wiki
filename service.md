# service

[[kubernetes]] service
label selector 를 통해서 load balancing 을 하며 endpoint 가 자동으로 붙는다
endpoint 가 생성되지 않으며 이를 위해서는 label selector 를 스펙 명시하지 않으면 된다
headless service 는 `sepc.clusterIP: None` 을 통해서 만들어지며 이 경우에 kube-proxy 에서 처리하지 않게된다.
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

## related
- [[kubernetes]]
