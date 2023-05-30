# eks immersion day

[[diary:2022-12-14]] 

- https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/ko-KR/100-scaling/200-cluster-scaling

- Container Overview
  - chroot -> pivot root
  - namespace -> 
  - cgroup -> 프로세스별로 가용 컴퓨팅 자원을 제어
- Dockerfile
  - from
  - label
  - expose
  - healthcheck
  - run
  - env
  - add
  - copy
  - entrypoint
  - cmd
  
  - add, copy
    - add 는 기능이 더 많다.
    - 압축된 파일을 add 하면서 해제도가능 
    - url 도 지원 가능 -> 빌드 캐시가 안될듯
  - entrypoint, cmd
    - entrypoint 는 변경되지 않으므로 보통 ex) node
    - cmd 는 오버라이딩이 됨, ex) node 의 파라메터
    
- docker expect 로 구조 확인 가능 **union mount system**
- docker network ls
  - bridge
  - host
  - container
  - none
- iptables > docker proxy 순서
- multi stage
  - build와 런타임에 필요한 디펜던시가 다른경우 처리
  
- kublet은 agent 형태로 따로 설치됨
- pod 아이피를 container는 공유 받는다 (sidecar 인경우 두 아이피가 같음)

- k8s
  - volume
    - emptyDir - 호스트 폴더 이용 파드 삭제시 제거 
    - hostPath - 노트 바뀔시에는 사용불가
    - pv - 노드가 바껴도 사용가능
    
  - pv 
    - dynamic pv -> pvc 에 의해서 pv를 생성하면서 할당
    
  - network
    - overlay network, 가상화가 될때 다른 노드의 가상화에서 ip가 겹치지 않도록 함
    
  - eth0, lo -> 도커 설치후 docker0 이 추가됨
```sh
docker network ls 
docker network ispect 
sudo su
iptables -t nat -S # port-forwarding 후 해보자
```
- role - rbac
  - rbac role -> namespace 단위
  - cluster role -> cluster 단위
  - service account -> pod 가 가지는 권한
  
- eks
  - cordon 해당 노드에 파드를 띄우지 않도록 한다. 스케줄링 제거
  - drain 해당 노드의 파드를 다른 파드로 이동시킨다.
  - pod disruption budge
  - 특정 파드 수는 유지하면서 파드 업데이트를 할 수 있다.
  - 특정 메타데이터를 주입하면 fargate 스케줄러에 의해서 따로 관리됨
  
- network
  - docker network -> local
  - overlay network -> node 간
  - cni
    - vpc - aws 는 vpc cni
      - secondary ip 가 인스턴스 타입 종속적으로 갯수가 정해짐
      - vcpu 의 캐파에 의해서도 제한됨
      - pod 생성 갯수가 제한 됨
      - secondary ip = network interface, private ip / network interface + @
      - vpc cni 플러그인을 사용하면 secondary ip * 16 이 가능
  - svc -> k-proxy -> iptables 수정
  - svc nodeport 시에 externalTrafficPolicy = local 로 설정, s-not 관련 설명인데 추후 찾아볼 것
  - lb 생성하면 aws lb 가 생성됨 cloud 벤더 별로 매니지머트 시스템이 존재
  - alb
    - instance mode -> node 에서 재 라우팅
    - ip mode -> 다이렉트로 파드, 성능 up
  - csi, conatiner storage interface
    - ebs 는 같은 az에서만 마운트 가능
- security
  - kms 적용하면 secret 이 암호호됨
  - guard duty
  - kubectl <-> iam authenticator client <-> sts
    - iam authenticator -> config map = aws-auth
  - irsa, iam role for service account
    - application 의 권한 - pod
    - ec2에 권한도 가능하나 pod 는 어떤 node 에 배포될지 정해져 있지 않다.
    - k8s sa 의 annotation 을 통해서 iam role 과 바인딩됨
    - pod
      - AWS_ROLE_ARN
      - AWS_WEB_IDENTITY_TOKEN_FILE
- 확장
  - cluster auto-scaler 노드 확장을 위한 플러그인
  - karpenter 더 유연, node group 에 묶이지 않는다.
  - kubetl edit 아래쪽에 status 는 etcd 에서 들어온다
  - ingress controller
    - ingress - alb
    - service - nlb
  - aws lb
    - instance(default) -> node port 로 보내고 알아서 route
    - ip -> alb에서 파드 자체로 라우팅
  - autoscaling 을 위해서는 metric-server 가 필요함
  
- kubelet 은 worker node 에서 직접 확인이 가능
  
- q&a
```yaml
alb.ingress.kubernetes.io/group.name: eks-demo-group
alb.ingress.kubernetes.io/group.order: '1'
```
  
- keyword 
  - netfilter
  - iptables
  - cni
  - eni 
  - aws load balancer controller 
  - csi 
  - bastion server 
  - argocd vs flux
  - eks blue prints
  - watch -n1
  - blue-green max-surge
  - siege 부하 테스트 k6 와 비교
  - https://codeberg.org/hjacobs/kube-ops-view
