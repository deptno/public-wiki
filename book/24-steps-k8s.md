# 24-steps-k8s|24단계 실습으로 정복하는 쿠버네티스

## 목차
[1부] 쿠버네티스의 개념과 설치, 기본 관리 방법 

▣ 01장: 쿠버네티스 개요와 클러스터 설치 
01. 쿠버네티스란? 
02. Kubespray를 이용해 3개의 노드로 구성된 클러스터 구축 
03. K3s를 이용해 단일 노드로 구성된 클러스터 구축 
04. 로컬호스트에서 원격 쿠버네티스 관리하기 
__1.4.1 로컬호스트에 쿠버네티스 명령어 실행 도구인 kubectl 설치 
__1.4.2 원격 클러스터 정보를 kubeconfig 파일에 등록 

▣ 02장: 효율적인 쿠버네티스 클러스터 관리를 위한 kubectl CLI 환경 최적화 
01. kubectl 자동 완성과 명령어 앨리어스 활용 
02. 쿠버네티스 krew를 이용한 플러그인 관리 
03. kube-ctx(컨텍스트), kube-ns(네임스페이스), kube-ps1(프롬프트) 활용 

▣ 03장: kubectl 명령어로 익히는 쿠버네티스의 주요 오브젝트 
01. NGINX 파드 실행과 배시 실행 
02. 디플로이먼트의 파드 개수 변경과 삭제 
03. 네임스페이스 생성 

▣ 04장: YAML 파일을 이용한 쿠버네티스 오브젝트 관리 
01. YAML 파일 익스포트 플러그인 kube-neat 설치 
02. YAML 파일을 이용한 파드 배포 
03. 쿠버네티스 YAML 템플릿 파일 검색 및 네이밍 규칙을 적용해 파일 저장하기 

▣ 05장: 쿠버네티스 트러블슈팅의 기본 프로세스 
01. 기본 에러 조치 프로세스의 이해: Apply - Get - Describe -Logs - Get Event 순으로 조치 
02. 장애 처리 사례: 호스트 노드의 파일 시스템 용량 초과 

▣ 06장: 헬름 기반으로 애플리케이션 설치하기 
01. 헬름의 주요 구성 요소: 헬름 차트, 헬름 리포지토리, 헬름 템플릿 
[[02]]. 헬름 차트를 이용한 NGINX 웹서버 설치 
__6.2.1 헬름을 이용한 애플리케이션 라이프사이클 관리 
__6.2.2 헬름 템플릿 변수 파일 사용하기 
__6.2.3 리소스 Requests/Limits 이해 

[2부] 쿠버네티스 네트워크 및 스토리지 인프라 환경 구성 

▣ 07장: 쿠버네티스 서비스 사용하기 
01. 클러스터 내부 파드 간 통신 
__7.1.1 클러스터IP 타입의 서비스 생성 
__7.1.2 서비스 디스커버리의 이해 
02. 쿠버네티스 DNS 기능 이해 
__7.2.1 CoreDNS 및 LocalDNS 설정 이해 
__7.2.2 쿠버네티스 DNS의 Search 옵션 설정 이해 
03. 클러스터 외부에서 내부의 파드 연결 
__7.3.1 노드포트 타입의 서비스 생성 
__7.3.2 부하분산 설정의 이해 

▣ 08장: MetalLB를 이용한 로드밸런서 타입 서비스 구축 
01. 헬름을 이용한 MetalLB 설치 
02. MetalLB 파드 아키텍처 확인 
__8.2.1 kubetail 설치 
__8.2.2 MetalLB 파드 로그 확인 
03. MetalLB 부하 테스트 및 고가용성 시나리오 검증 
__8.3.1 k6를 이용한 부하 테스트 
__8.3.2 노드 장애 시의 서비스 다운시간 측정 

▣ 09장: Traefik을 이용한 쿠버네티스 인그레스 구축 
01. Traefik 인그레스 컨트롤러 설치 
02. 인그레스 테스트용 애플리케이션 설치 
03. Traefik 인그레스 설정 테스트 
__9.3.1 Traefik CRD를 이용한 인그레스 설정의 이해 
__9.3.2 가상 호스트와 URL 경로에 따른 서비스 분기 
__9.3.3 사용자 SSL/TLS 인증서 적용 

▣ 10장: 쿠버네티스 스토리지 
01. 쿠버네티스 영구볼륨, PVC, 스토리지 클래스의 이해 
02. OpenEBS 로컬 호스트패스 설치 
03. 스토리지 클래스를 이용한 PVC 및 영구볼륨 사용 
04. 사용자 스토리지 클래스를 지정해 헬름 차트 MySQL 설치하기 
05. 로컬 호스트패스 스토리지 클래스의 장점 및 제약 사항 
__10.5.1 뛰어난 IOPS 성능 - Kubestr을 이용한 성능 측정 
__10.5.2 스토리지 고가용성 구성 제약 - 노드 제거 테스트 

▣ 11장: 스토리지 볼륨 스냅샷 사용하기 
01. rook-ceph를 이용한 쿠버네티스 셰프 스토리지 설치 
02. 워드프레스 블로그 애플리케이션의 설치 및 스냅샷 생성 
__11.2.1 워드프레스 애플리케이션 설치 
__11.2.2 스토리지 볼륨 스냅샷 생성 
03. 스냅샷을 이용한 애플리케이션 데이터 복구 
04. rook-ceph 스토리지의 가용성 테스트 및 IOPS 성능 측정 

▣ 12장: 쿠버네티스 환경에서 공유 파일 스토리지 사용하기 
01. 루크-셰프 이용한 공유 파일 스토리지 설치 
02. 여러 파드에서 동시에 단일 파일 스토리지에 마운트하기 
03. 스토리지 고가용성 테스트 

[03부] 쿠버네티스 애플리케이션 배포 인프라 구축 

▣ 13장: 하버를 이용한 로컬 컨테이너 이미지 저장소 구축 
01. 헬름 차트를 이용한 하버 설치 
02. 로컬 컨테이너 이미지를 원격 하버 이미지 저장소로 업로드 
03. 쿠버네티스 YAML 파일의 컨테이너 이미지 저장소 주소를 로컬 하버로 변경 
04. 컨테이너 이미지 업로드 시 자동으로 이미지에 대한 보안 스캔 기능 활성화 

▣ 14장: 깃랩을 이용한 로컬 Git 소스 저장소 구축 
01. 헬름 차트 기반으로 깃랩 설치 
02. 로컬 쿠버네티스 YAML 소스코드를 원격 깃랩 저장소에 동기화 

▣ 15장: 아르고시디를 활용한 깃옵스 시스템 구축 
01. 헬름 차트를 이용한 아르고시디 설치 
02. 아르고시디를 이용한 래빗엠큐 헬름 애플리케이션 배포 
03. GitOps 실습: 클러스터 설정 내역 변경과 깃 저장소 자동 반영 

[04부] 쿠버네티스 모니터링 및 로깅 시스템 구축 

▣ 16장: 간단하게 사용할 수 있는 쿠버네티스 모니터링 도구 
01. 메트릭 서버를 이용한 파드 및 노드의 리소스 사용량 확인 
02. 명령어 기반 쿠버네티스 모니터링 도구 k9s 

▣ 17장: 프로메테우스 - 쿠버네티스 모니터링 시스템 
01. 헬름 차트 기반의 프로메테우스-스택 설치 
02. 프로메테우스 아키텍처 
03. 프로메테우스 웹 UI 활용: 상세 설정 내역 확인 및 모니터링 그래프 확인하기 

▣ 18장: 그라파나 - 쿠버네티스 모니터링 대시보드 
01. 프로메테우스-스택에 사전 포함된 그라파나 대시보드 사용하기 
__18.1.1 그라파나의 기본 사용법 
__18.1.2 기본 대시보드 확인 
02. 그라파나 공식 홈페이지의 템플릿 대시보드 추가하기 
03. NGINX 애플리케이션 모니터링 대시보드 추가: 프로메테우스 서비스모니터와 PromQL의 기본 사용법 

▣ 19장: 얼럿매니저 - 쿠버네티스 경보 서비스 
01. 프로메테우스와 얼럿매니저의 시스템 경보 기능 
02. 시스템 경고 메시지 전달을 위한 슬랙 채널 및 웹훅 URL 생성 
03. 얼럿매니저 설정 파일에 슬랙 웹훅 URL 등록 
04. 얼럿매니저 기능 검증 
__19.4.1 임의의 노드를 다운시킨 후 슬랙 채널 메시지를 확인 
__19.4.2 시스템 경고 정책(prometheusrules)의 상세 내용 확인 
__19.4.3 얼럿매니저의 일시 중지 기능 사용하기 
05. 사용자 정의 prometheusrules 정책 설정: 파일시스템 사용률 80% 초과 시 시스템 경고 발생시키기 

▣ 20장: 로키 - 쿠버네티스 로깅 시스템 
01. 로키 시스템의 구조와 설치 
02. 로키를 이용한 쿠버네티스 로그 검색 
03. LogQL 사용법 익히기: 특정 네임스페이스의 로그 및 정규 표현식을 이용한 로그 검색 

[05부] 쿠버네티스 보안 시스템 구축 

21장: 쿠버네티스 보안 도구 활용 
01. kubescape - NSA/CISA 프레임워크 기반 보안 점검 도구 
02. 폴라리스 활용 
__21.2.1 레디스 헬름 차트 설치 
__21.2.2 폴라리스 설치 및 레디스의 보안 취약점 확인 

▣ 22장: 역할 기반 접근 제어(RBAC) 설정 
01. Role/RoleBinding과 ClusterRole/ClusterRoleBinding 이해 
02. ServiceAccount와 User, kubeconfig 파일 이해: 특정 네임스페이스 권한만 가지는 사용자 생성 
03. 멀티테넌시 환경의 쿠버네티스 구성: 사용자별 네임스페이스 단위의 권한 제한 

[06부] 실제 서비스 운영에 필요한 기술 

▣ 23장: 애플리케이션 부하 테스트와 고가용성 테스트 
01. 데모 용도의 방명록 서비스 설치 
02. k6를 이용한 웹 부하 테스트 
03. 애플리케이션 고가용성 테스트 
__23.3.1 특정 노드 내에서 실행 중인 모든 파드 종료하기 
__23.3.2 파드 삭제 및 노드 종료 시 서비스 이상 여부 검증 

▣ 24장: 쿠버네티스 노드 변경과 추가 
01. 컨트롤 플레인 노드 변경과 워커 노드 추가

## 실습 

1. 01장: 쿠버네티스 개요와 클러스터 설치  
  - **클러스터 설치**
    1. [[../multipass]] 설정
      - [X] DONE: 인스턴스 stop 후 start 했을때 api-server 가 먹통
        start 후 시간이 꽤 흘러야함, 20분정도 후에 테스트 해보니 됨
      - instace 3대시작 kube0{0,1,2} [[../kubespray]]
    2. [[../kubespray]] 
      - multipass ls
      - multipass shell kube00
        - vi /etc/hosts
          - kube00
```txt
127.0.1.1 kube00 kube00 
127.0.1.1 localhost
```
        - [ ] TODO: ip 자체를 할당할 경우 kubespray 설치 실패 재시도 필요
        - ssh kube01
    3. 타 인스턴스 접속을 위한 /etc/hosts
```txt
127.0.1.1 kube00 kube00
192.168.64.5 kube00 kube00
192.168.64.6 kube01 kube01
192.168.64.7 kube02 kube02
127.0.0.1 localhost
```
    4. kubectl 을 통해서 컨피그 등 확인, 이미 해서 패스
2. 02장: 효율적인 쿠버네티스 클러스터 관리를 위한 kubectl CLI 환경 최적화 
  **로컬 os 설정**
  1. shell autocomplete 설정
  2. [[../krew]] k8s plugin manager 설정
    - ns
```shell
kubectl krew install ctx
kubectl ctx # listing
kubectl ctx [context-name] # 컨텍스트 변경
```
    - ctx
```shell
kubectl krew install ns
kubectl ns # listing
kubectl ns [namespace-name] # 네임스페이스 변경
```
  3. [[kube-ps1]] shell 에서 상태를 보여줌
    **설치하지 않음, tmux 플러그인으로 대체했음**
3. 03장: kubectl 명령어로 익히는 쿠버네티스의 주요 오브젝트 
  - pod
```shell
$ k ns
Context "kubernetes-admin@cluster.local" modified.
Active namespace is "default".
$ k run nginx --image=nginx
pod/nginx created
$ kgp
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          10s
$ k run nginx01 --image=nginx
pod/nginx01 created
$ kgpwide
NAME      READY   STATUS              RESTARTS   AGE     IP              NODE     NOMINATED NODE   READINESS GATES
nginx     1/1     Running             0          2m12s   10.233.85.197   kube02   <none>           <none>
nginx01   0/1     ContainerCreating   0          3s      <none>          kube01   <none>           <none>
```
    - 파드내 실행 프로세스 확인
```shell
$ k exec -it nginx -- bash
root@nginx:/# apt -y update && apt -y install procps # ps 실행을 위해 설치
root@nginx:/# ps aux
```
    - 개발 파드가 사용하는 볼륨은 호스트 노드의 /var/lib/containers/[pod-name]
      - [ ] 막상 들어가보니 디렉토리 자체가 존재하지 않음
        - /var/lib/containerd/* 은 존재하나 pod(nginx) 는 존재하지 않았음
  - deployment
```shell
$ k create deployment httpd --image=httpd
deployment.apps/httpd created
$ kgpwide                         ok  1.59.0 rust  kubernetes-admin@cluster.local kube  15:31:47
NAME                     READY   STATUS              RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
httpd-65bfffd87f-59rpd   0/1     ContainerCreating   0          3s    <none>          kube00   <none>           <none>
nginx                    1/1     Running             0          17m   10.233.85.197   kube02   <none>           <none>
nginx01                  1/1     Running             0          15m   10.233.72.132   kube01   <none>           <none>
$ kgpwide                         ok  1.59.0 rust  kubernetes-admin@cluster.local kube  15:31:50
NAME                     READY   STATUS              RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
httpd-65bfffd87f-59rpd   0/1     ContainerCreating   0          6s    <none>          kube00   <none>           <none>
nginx                    1/1     Running             0          17m   10.233.85.197   kube02   <none>           <none>
nginx01                  1/1     Running             0          15m   10.233.72.132   kube01   <none>           <none>
$ kgpwide                         ok  1.59.0 rust  kubernetes-admin@cluster.local kube  15:31:53
NAME                     READY   STATUS    RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
httpd-65bfffd87f-59rpd   1/1     Running   0          10s   10.233.102.7    kube00   <none>           <none>
nginx                    1/1     Running   0          18m   10.233.85.197   kube02   <none>           <none>
nginx01                  1/1     Running   0          15m   10.233.72.132   kube01   <none>           <none>
$ k scale deployment httpd --replicas 10
deployment.apps/httpd scaled
$ kgpwide                         ok  1.59.0 rust  kubernetes-admin@cluster.local kube  15:32:14
NAME                     READY   STATUS              RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
httpd-65bfffd87f-226lx   0/1     ContainerCreating   0          3s    <none>          kube00   <none>           <none>
httpd-65bfffd87f-4lm5x   0/1     ContainerCreating   0          3s    <none>          kube01   <none>           <none>
httpd-65bfffd87f-59rpd   1/1     Running             0          30s   10.233.102.7    kube00   <none>           <none>
httpd-65bfffd87f-5nvp4   0/1     ContainerCreating   0          3s    <none>          kube02   <none>           <none>
httpd-65bfffd87f-9fb55   0/1     ContainerCreating   0          3s    <none>          kube02   <none>           <none>
httpd-65bfffd87f-d5qdk   0/1     ContainerCreating   0          3s    <none>          kube01   <none>           <none>
httpd-65bfffd87f-d9ztz   0/1     ContainerCreating   0          3s    <none>          kube01   <none>           <none>
httpd-65bfffd87f-q66vs   0/1     ContainerCreating   0          3s    <none>          kube02   <none>           <none>
httpd-65bfffd87f-r5fgn   0/1     ContainerCreating   0          3s    <none>          kube00   <none>           <none>
httpd-65bfffd87f-zr2zp   0/1     ContainerCreating   0          3s    <none>          kube00   <none>           <none>
nginx                    1/1     Running             0          18m   10.233.85.197   kube02   <none>           <none>
nginx01                  1/1     Running             0          16m   10.233.72.132   kube01   <none>           <none>
$ k scale deployment httpd --replicas 0
deployment.apps/httpd scaled
$ kgpwide                         ok  1.59.0 rust  kubernetes-admin@cluster.local kube  15:35:06
NAME      READY   STATUS    RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
nginx     1/1     Running   0          21m   10.233.85.197   kube02   <none>           <none>
nginx01   1/1     Running   0          19m   10.233.72.132   kube01   <none>           <none>
$ k scale deployment httpd --replicas 1
deployment.apps/httpd scaled
$ kgpwide                         ok  1.59.0 rust  kubernetes-admin@cluster.local kube  15:35:15
NAME                     READY   STATUS              RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
httpd-65bfffd87f-jtc8x   0/1     ContainerCreating   0          2s    <none>          kube00   <none>           <none>
nginx                    1/1     Running             0          21m   10.233.85.197   kube02   <none>           <none>
nginx01                  1/1     Running             0          19m   10.233.72.132   kube01   <none>           <none>
$ k delete pod httpd-65bfffd87f-jtc8x
pod "httpd-65bfffd87f-jtc8x" deleted
$ kgpwide                         ok  1.59.0 rust  kubernetes-admin@cluster.local kube  15:37:18
NAME                     READY   STATUS    RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
httpd-65bfffd87f-x7ddc   1/1     Running   0          6s    10.233.85.201   kube02   <none>           <none>
nginx                    1/1     Running   0          23m   10.233.85.197   kube02   <none>           <none>
nginx01                  1/1     Running   0          21m   10.233.72.132   kube01   <none>           <none>
```
    - 파드를 삭제해도 deployment 의 replicas 로 인해 파드가 자동 재생성 되는 것 확인
  - 네임스페이스 생성
```shell
$ k create ns default01                                                                  ok  kubernetes-admin@cluster.local kube  15:39:28
namespace/default01 created
$ k ns default                                                                           ok  kubernetes-admin@cluster.local kube  15:39:40
Context "kubernetes-admin@cluster.local" modified.
Active namespace is "default".
$ k delete pod nginx{,01}                                                             1 err  kubernetes-admin@cluster.local kube  15:40:20
pod "nginx" deleted
pod "nginx01" deleted
$ k delete deployments.apps httpd                                                        ok  kubernetes-admin@cluster.local kube  15:40:27
deployment.apps "httpd" deleted
$ kgpwide                                                                                ok  kubernetes-admin@cluster.local kube  15:40:39
No resources found in default namespace.
```
    - 네임스페이스 내에서는 같은 이름의 파드를 재생성할 수 없다
```shell
$ k run nginx --image=nginx                                                              ok  kubernetes-admin@cluster.local kube  15:40:41
pod/nginx created
$ k run nginx --image=nginx                                                              ok  kubernetes-admin@cluster.local kube  15:41:28
Error from server (AlreadyExists): pods "nginx" already exists
$ k ns default01                                                                      1 err  kubernetes-admin@cluster.local kube  15:41:29
Context "kubernetes-admin@cluster.local" modified.
Active namespace is "default01".
$ k run nginx --image=nginx                                                    ok  kubernetes-admin@cluster.local/default01 kube  15:41:57
pod/nginx created
```
    - 다른 네임스페이스 파드간 통신
      네임스페이스는 네트워크 수준의 분리가 아니다
```shell
 ~  kgpwide -n default01                                                                                                   INT  9s  15:45:13
 ~  kgpwide -n default                                                      INT  9s  kubernetes-admin@cluster.local/default01 kube  15:45:13
NAME    READY   STATUS    RESTARTS   AGE     IP              NODE     NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          3m53s   10.233.85.202   kube02   <none>           <none>
 ~  kgpwide -n default01                                                         ok  kubernetes-admin@cluster.local/default01 kube  15:45:21
NAME    READY   STATUS    RESTARTS   AGE     IP              NODE     NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          3m19s   10.233.72.136   kube01   <none>           <none>
 ~  k exec -it nginx -- bash                                                     ok  kubernetes-admin@cluster.local/default01 kube  15:45:22
root@nginx:/# apt-get update -y && apt-get install iputils-ping -y
root@nginx:/# ping 10.233.85.202 -c 1
PING 10.233.85.202 (10.233.85.202) 56(84) bytes of data.
64 bytes from 10.233.85.202: icmp_seq=1 ttl=62 time=0.632 ms
root@nginx:/# ping 10.233.72.136 -c 1
PING 10.233.72.136 (10.233.72.136) 56(84) bytes of data.
64 bytes from 10.233.72.136: icmp_seq=1 ttl=64 time=0.035 ms
--- 10.233.72.136 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.035/0.035/0.035/0.000 ms
root@nginx:/#
```
04. 04장: YAML 파일을 이용한 쿠버네티스 오브젝트 관리 
  - neat 설치, 상태나 기본값을 제거한 이력 관리용 yaml 을 추출
```shell
$ k run busybox --image=busybox                                                          ok  kubernetes-admin@cluster.local kube  16:00:14
pod/busybox created
$ kgpwide # busybox 가 실행 후 바로 완료됨                                                                                ok  kubernetes-admin@cluster.local kube  16:00:52
NAME      READY   STATUS      RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
busybox   0/1     Completed   0          5s    10.233.102.14   kube00   <none>           <none>
nginx     1/1     Running     0          19m   10.233.85.202   kube02   <none>           <none>
$ kgp busybox -o yaml                                
# ... verbose yaml
```
    - neat 설치
```shell
$ k krew install neat                                                                    ok  kubernetes-admin@cluster.local kube  16:01:58
Updated the local copy of plugin index.
Installing plugin: neat
Installed plugin: neat
\
 | Use this plugin:
 |      kubectl neat
 | Documentation:
 |      https://github.com/itaysk/kubectl-neat
/
WARNING: You installed plugin "neat" from the krew-index plugin repository.
   These plugins are not audited for security by the Krew maintainers.
   Run them at your own risk.
$ kgp busybox -o yaml | k neat                                                          INT  kubernetes-admin@cluster.local kube  16:02:17
apiVersion: v1
kind: Pod
metadata:
# ... yaml
$ kgp busybox -o yaml | k neat > busybox-pod.yaml
$ vi busybox-pod.yaml
```
# ... 바로 완료되지 않고 살아있게 만들기 위해서 command 추가
spec:
  containers:
  - image: busybox
    name: busybox
    command:
    - "/bin/sh"
    - "-c"
    - "sleep inf"
```yaml
```shell
 ~/w/st/k8s-24-steps  kaf busybox-pod.yaml
 ~/w/st/k8s-24-steps  kaf busybox-pod.yaml                                            ok  11s  kubernetes-admin@cluster.local kube  16:11:46
pod/busybox created
 ~/w/st/k8s-24-steps  kgpwide                                                              ok  kubernetes-admin@cluster.local kube  16:12:04
NAME      READY   STATUS    RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
busybox   1/1     Running   0          3s    10.233.102.16   kube00   <none>           <none>
nginx     1/1     Running   0          30m   10.233.85.202   kube02   <none>           <none>
 ~/w/st/k8s-24-steps  vi busybox-pod.yaml                                                  ok  kubernetes-admin@cluster.local kube  16:12:04
```
```yaml
# ... resources 추가
    command:
    - "/bin/sh"
    - "-c"
    - "sleep inf"
    resources:
      limits:
        memory: 512Mi
      requests:
        memory: 128Mi
```
  - 네이밍 규칙
    - 거의 모든 k8s 오브젝트는AKMS(apiVersion,kind,metadata,spec)필 을 포함
    - 저자의 규칙
      - [app-name]-[option]-[object-name].yaml
      - 디렉토리
        1. Pods
        2. Deployments
        3. StatefulSet
        4. Daemonset
        5. JobCronjob
        6. volume
        7. network
05. 05장: 쿠버네티스 트러블슈팅의 기본 프로세스 
  - AGDLG
    1. Apply
    2. Get
    3. Describe
    4. Logs
    5. Get Event
  - 없는 이미지를 사용한 트러블슈팅 시나리오
```shell
 ~/workspace/study/k8s-24-steps  vi nginx-error-pod.yaml                                                                        ok  16:25:08
 ~/workspace/study/k8s-24-steps  cat nginx-error-pod.yaml                                                               ok  1m 35s  16:26:54
apiVersion: v1
kind: Pod
metadata:
  name: nginx-19
spec:
  container:
    name: nginx-pod
    image: nginx:1.19.19 # 존재하지 않는 이미지
 ~/w/st/k8s-24-steps  kaf nginx-error-pod.yaml                                             ok  kubernetes-admin@cluster.local kube  16:27:01
error: error validating "nginx-error-pod.yaml": error validating data: [ValidationError(Pod.spec): unknown field "container" in io.k8s.api.core.v1.PodSpec, ValidationError(Pod.spec): missing required field "containers" in io.k8s.api.core.v1.PodSpec]; if you choose to ignore these errors, turn validation off with --validate=false
 ~/workspace/study/k8s-24-steps  !v                                                                                          1 err  16:27:06
 ~/workspace/study/k8s-24-steps  vi nginx-error-pod.yaml                                                                     1 err  16:27:16
 ~/workspace/study/k8s-24-steps                                                                                            ok  10s  16:27:26
 ~/workspace/study/k8s-24-steps                                                                                            ok  10s  16:27:26
 ~/workspace/study/k8s-24-steps  !v                                                                                        ok  10s  16:27:26
 ~/workspace/study/k8s-24-steps  vi nginx-error-pod.yaml                                                                        ok  16:27:30
 ~/workspace/study/k8s-24-steps  cat nginx-error-pod.yaml                                                                       ok  16:27:31
apiVersion: v1
kind: Pod
metadata:
  name: nginx-19
spec:
  containers:
    - name: nginx-pod
      image: nginx:1.19.19 # 존재하지 않는 이미지
 ~/w/st/k8s-24-steps  kaf nginx-error-pod.yaml                                             ok  kubernetes-admin@cluster.local kube  16:27:34
pod/nginx-19 created
 ~/w/st/k8s-24-steps  kgpwide                                                              ok  kubernetes-admin@cluster.local kube  16:27:39
NAME       READY   STATUS         RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
busybox    1/1     Running        0          15m   10.233.102.16   kube00   <none>           <none>
nginx      1/1     Running        0          46m   10.233.85.202   kube02   <none>           <none>
nginx-19   0/1     ErrImagePull   0          7s    10.233.85.203   kube02   <none>           <none>
 ~/w/st/k8s-24-steps  kdp nginx-19                                                         ok  kubernetes-admin@cluster.local kube  16:27:46
Name:         nginx-19
Namespace:    default
Priority:     0
Node:         kube02/192.168.64.7
Start Time:   Tue, 20 Dec 2022 16:27:39 +0900
Labels:       <none>
Annotations:  cni.projectcalico.org/containerID: a4249b8b4f1ad3cc1dc92af29a9ea267e7bcb14a88a678ab18d6508c93c4ece8
              cni.projectcalico.org/podIP: 10.233.85.203/32
              cni.projectcalico.org/podIPs: 10.233.85.203/32
Status:       Pending
IP:           10.233.85.203
IPs:
  IP:  10.233.85.203
Containers:
  nginx-pod:
    Container ID:
    Image:          nginx:1.19.19
    Image ID:
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       ErrImagePull
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-9bxfp (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             False
  ContainersReady   False
  PodScheduled      True
Volumes:
  kube-api-access-9bxfp:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                From               Message
  ----     ------     ----               ----               -------
  Normal   Scheduled  70s                default-scheduler  Successfully assigned default/nginx-19 to kube02
  Normal   Pulling    24s (x3 over 70s)  kubelet            Pulling image "nginx:1.19.19"
  Warning  Failed     21s (x3 over 67s)  kubelet            Failed to pull image "nginx:1.19.19": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/library/nginx:1.19.19": failed to resolve reference "docker.io/library/nginx:1.19.19": docker.io/library/nginx:1.19.19: not found
  Warning  Failed     21s (x3 over 67s)  kubelet            Error: ErrImagePull
  Normal   BackOff    11s (x3 over 67s)  kubelet            Back-off pulling image "nginx:1.19.19"
  Warning  Failed     11s (x3 over 67s)  kubelet            Error: ImagePullBackOff
 ~/workspace/study/k8s-24-steps  vi nginx-error-pod.yaml                                                                        ok  16:28:49
 ~/workspace/study/k8s-24-steps  cat nginx-error-pod.yaml                                                                  ok  10s  16:29:35
apiVersion: v1
kind: Pod
metadata:
  name: nginx-19
spec:
  containers:
    - name: nginx-pod
      image: nginx:1.19
 ~/w/st/k8s-24-steps  kaf nginx-error-pod.yaml                                             ok  kubernetes-admin@cluster.local kube  16:29:39
pod/nginx-19 configured
 ~/w/st/k8s-24-steps  kgpwide                                                              ok  kubernetes-admin@cluster.local kube  16:30:03
NAME       READY   STATUS             RESTARTS   AGE     IP              NODE     NOMINATED NODE   READINESS GATES
busybox    1/1     Running            0          18m     10.233.102.16   kube00   <none>           <none>
nginx      1/1     Running            0          48m     10.233.85.202   kube02   <none>           <none>
nginx-19   0/1     ImagePullBackOff   0          2m28s   10.233.85.203   kube02   <none>           <none>
 ~/w/st/k8s-24-steps  kgpwide                                                              ok  kubernetes-admin@cluster.local kube  16:30:07
NAME       READY   STATUS             RESTARTS   AGE     IP              NODE     NOMINATED NODE   READINESS GATES
busybox    1/1     Running            0          18m     10.233.102.16   kube00   <none>           <none>
nginx      1/1     Running            0          48m     10.233.85.202   kube02   <none>           <none>
nginx-19   0/1     ImagePullBackOff   0          2m30s   10.233.85.203   kube02   <none>           <none>
 ~/w/st/k8s-24-steps  k delete pod nginx19                                                 ok  kubernetes-admin@cluster.local kube  16:30:09
Error from server (NotFound): pods "nginx19" not found
 ~/w/st/k8s-24-steps  k delete pod nginx-19                                             1 err  kubernetes-admin@cluster.local kube  16:30:14
pod "nginx-19" deleted
 ~/w/st/k8s-24-steps  kaf nginx-error-pod.yaml                                             ok  kubernetes-admin@cluster.local kube  16:30:17
pod/nginx-19 created
 ~/w/st/k8s-24-steps  kgpwide                                                              ok  kubernetes-admin@cluster.local kube  16:30:20
NAME       READY   STATUS    RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
busybox    1/1     Running   0          18m   10.233.102.16   kube00   <none>           <none>
nginx      1/1     Running   0          48m   10.233.85.202   kube02   <none>           <none>
nginx-19   1/1     Running   0          6s    10.233.85.204   kube02   <none>           <none>
 ~/w/st/k8s-24-steps  klf nginx-19                                                         ok  kubernetes-admin@cluster.local kube  16:30:26
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
^C
```
    1. get 
    2. describe 를 통해서 디테일한 이슈 확인
    3. 정상작동을 log 통해서 확인
       label 로 필터링하면 여러 파드의 로그를 함게 확인할 수 잇는 것으로 보인다
```shell
 ~/w/study/k8s-24-steps  kl1h -l component=kube-apiserver -n kube-system                                          ok  kubernetes-admin@cluster.local kube  16:38:59
I1220 06:39:40.498414       1 controller.go:616] quota admission added evaluator for: serviceaccounts
I1220 06:39:40.478986       1 controller.go:616] quota admission added evaluator for: namespaces
I1220 07:32:11.071205       1 trace.go:205] Trace[908013652]: "Get" url:/api/v1/namespaces/default/pods/nginx-19/log,user-agent:kubectl/v1.22.5 (darwin/arm64) kubernetes/5c99e2a,audit-id:cf9bb093-8905-4708-a974-2ee9fbc2ae83,client:192.168.64.1,accept:application/json, */*,protocol:HTTP/2.0 (20-Dec-2022 07:31:25.959) (total time: 45111ms):
Trace[908013652]: ---"Writing http response done" 45107ms (07:32:11.071)
Trace[908013652]: [45.111899611s] [45.111899611s] END
```
    - events
```shell
$ k get events
$ k get events -n kube-system
$ k get events -A # all namespaces
```
  - 호스트 노드의 용량 초과 시나리오 트러블슈팅
```shell
 ~/workspace/study/k8s-24-steps  vim busybox-deploy.yaml                                                                        ok  16:55:20
 ~/workspace/study/k8s-24-steps  cat busybox-deploy.yaml                                                                     1 err  16:55:26
apiVersion: apps/v1
kind: Deployment
metadata:
  name: busybox
  labels:
    app: busybox
spec:
  replicas: 10
  selector:
    matchLabels:
      app: busybox
  template:
    metadata:
      labels:
        app: busybox
    spec:
      containers:
        - name: busybox
          image: busybox
          command:
          - "/bin/sh"
          - "-c"
          - "sleep inf"
 ~/w/st/k8s-24-steps  kaf busybox-deploy.yaml                                              ok  kubernetes-admin@cluster.local kube  16:55:27
deployment.apps/busybox created
 ~/w/st/k8s-24-steps  kgpwide                                                              ok  kubernetes-admin@cluster.local kube  16:55:29
NAME                      READY   STATUS              RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
busybox-9ff887cc8-487jr   1/1     Running             0          8s    10.233.85.206   kube02   <none>           <none>
busybox-9ff887cc8-cgfwf   0/1     ContainerCreating   0          8s    <none>          kube01   <none>           <none>
busybox-9ff887cc8-ctqt4   1/1     Running             0          8s    10.233.72.138   kube01   <none>           <none>
busybox-9ff887cc8-dh7kh   1/1     Running             0          8s    10.233.72.137   kube01   <none>           <none>
busybox-9ff887cc8-f7gtp   1/1     Running             0          8s    10.233.85.207   kube02   <none>           <none>
busybox-9ff887cc8-hwwtn   0/1     ContainerCreating   0          8s    <none>          kube01   <none>           <none>
busybox-9ff887cc8-l7fdb   1/1     Running             0          8s    10.233.85.205   kube02   <none>           <none>
busybox-9ff887cc8-mv8v4   1/1     Running             0          8s    10.233.102.17   kube00   <none>           <none>
busybox-9ff887cc8-nnkrx   1/1     Running             0          8s    10.233.102.18   kube00   <none>           <none>
busybox-9ff887cc8-rtwtm   1/1     Running             0          8s    10.233.102.19   kube00   <none>           <none>
nginx                     1/1     Running             0          74m   10.233.85.202   kube02   <none>           <none>
nginx-19                  1/1     Running             0          25m   10.233.85.204   kube02   <none>           <none>
```
    - kube01 에 접속해서 큰 용량의 파일을 생성  
```shell
 ~/workspace/study/k8s-24-steps  m shell kube01                                                                                 ok  16:55:37
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-56-generic aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Dec 20 16:59:06 KST 2022

  System load:             0.7626953125
  Usage of /:              74.7% of 9.52GB
  Memory usage:            24%
  Swap usage:              0%
  Processes:               145
  Users logged in:         0
  IPv4 address for enp0s1: 192.168.64.6
  IPv6 address for enp0s1: fdec:f006:592:f10d:5054:ff:fe8d:ef7f

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

9 updates can be applied immediately.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Tue Dec 20 15:22:10 2022 from 192.168.64.1
ubuntu@kube01:~$ df -h /
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       9.6G  7.2G  2.4G  75% /
ubuntu@kube01:~$ fallocate -l 2.2g 2.2g-file
ubuntu@kube01:~$ df -h  /
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       9.6G  8.8G  820M  92% /
ubuntu@kube01:~$ exit
```
    - 용량 부족으로 인해 kube01 노드의 파드들이 에러를 내고 다른 노드에서 ContainerCreating 이 발생하는 것을 확인
```shell
 ~/w/study/k8s-24-steps  kgpwide                                                                                  ok  kubernetes-admin@cluster.local kube  17:05:25
NAME                      READY   STATUS              RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
busybox-9ff887cc8-487jr   1/1     Running             0          10m   10.233.85.206   kube02   <none>           <none>
busybox-9ff887cc8-cgfwf   0/1     Error               0          10m   10.233.72.140   kube01   <none>           <none>
busybox-9ff887cc8-ctqt4   0/1     Error               0          10m   10.233.72.138   kube01   <none>           <none>
busybox-9ff887cc8-dh7kh   0/1     Error               0          10m   10.233.72.137   kube01   <none>           <none>
busybox-9ff887cc8-f7gtp   1/1     Running             0          10m   10.233.85.207   kube02   <none>           <none>
busybox-9ff887cc8-hwwtn   1/1     Running             0          10m   10.233.72.139   kube01   <none>           <none>
busybox-9ff887cc8-l7fdb   1/1     Running             0          10m   10.233.85.205   kube02   <none>           <none>
busybox-9ff887cc8-mv8v4   1/1     Running             0          10m   10.233.102.17   kube00   <none>           <none>
busybox-9ff887cc8-nnkrx   1/1     Running             0          10m   10.233.102.18   kube00   <none>           <none>
busybox-9ff887cc8-rn2v6   0/1     ContainerCreating   0          2s    <none>          kube00   <none>           <none>
busybox-9ff887cc8-rtwtm   1/1     Running             0          10m   10.233.102.19   kube00   <none>           <none>
busybox-9ff887cc8-sdgld   1/1     Running             0          33s   10.233.85.208   kube02   <none>           <none>
busybox-9ff887cc8-swz78   1/1     Running             0          64s   10.233.102.20   kube00   <none>           <none>
nginx                     1/1     Running             0          84m   10.233.85.202   kube02   <none>           <none>
nginx-19                  1/1     Running             0          35m   10.233.85.204   kube02   <none>           <none>
```
```shell
 ~/w/study/k8s-24-steps  k get events | head -n 20                                                             1 err  kubernetes-admin@cluster.local kube  17:14:15
LAST SEEN   TYPE      REASON                 OBJECT                         MESSAGE
18m         Normal    Scheduled              pod/busybox-9ff887cc8-487jr    Successfully assigned default/busybox-9ff887cc8-487jr to kube02
18m         Normal    Pulling                pod/busybox-9ff887cc8-487jr    Pulling image "busybox"
18m         Normal    Pulled                 pod/busybox-9ff887cc8-487jr    Successfully pulled image "busybox" in 5.44378738s
18m         Normal    Created                pod/busybox-9ff887cc8-487jr    Created container busybox
18m         Normal    Started                pod/busybox-9ff887cc8-487jr    Started container busybox
18m         Normal    Scheduled              pod/busybox-9ff887cc8-cgfwf    Successfully assigned default/busybox-9ff887cc8-cgfwf to kube01
18m         Normal    Pulling                pod/busybox-9ff887cc8-cgfwf    Pulling image "busybox"
18m         Normal    Pulled                 pod/busybox-9ff887cc8-cgfwf    Successfully pulled image "busybox" in 8.104276201s
18m         Normal    Created                pod/busybox-9ff887cc8-cgfwf    Created container busybox
18m         Normal    Started                pod/busybox-9ff887cc8-cgfwf    Started container busybox
10m         Warning   Evicted                pod/busybox-9ff887cc8-cgfwf    The node was low on resource: ephemeral-storage. Container busybox was using 36Ki, which exceeds its request of 0.
10m         Normal    Killing                pod/busybox-9ff887cc8-cgfwf    Stopping container busybox
10m         Warning   ExceededGracePeriod    pod/busybox-9ff887cc8-cgfwf    Container runtime did not kill the pod within specified grace period.
18m         Normal    Scheduled              pod/busybox-9ff887cc8-ctqt4    Successfully assigned default/busybox-9ff887cc8-ctqt4 to kube01
18m         Normal    Pulling                pod/busybox-9ff887cc8-ctqt4    Pulling image "busybox"
18m         Normal    Pulled                 pod/busybox-9ff887cc8-ctqt4    Successfully pulled image "busybox" in 5.48281798s
18m         Normal    Created                pod/busybox-9ff887cc8-ctqt4    Created container busybox
18m         Normal    Started                pod/busybox-9ff887cc8-ctqt4    Started container busybox
9m9s        Warning   Evicted                pod/busybox-9ff887cc8-ctqt4    The node was low on resource: ephemeral-storage. Container busybox was using 36Ki, which exceeds its request of 0.
 ~/w/st/k8s-24-steps  kdno kube01 # kubectl describe node kube01                                           PIPE|0 ok  kubernetes-admin@cluster.local kube  17:14:20
Name:               kube01
Roles:              control-plane
Labels:             beta.kubernetes.io/arch=arm64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=arm64
                    kubernetes.io/hostname=kube01
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/control-plane=
                    node.kubernetes.io/exclude-from-external-load-balancers=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    projectcalico.org/IPv4Address: 192.168.64.6/24
                    projectcalico.org/IPv4VXLANTunnelAddr: 10.233.72.128
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sun, 27 Nov 2022 17:38:40 +0900
Taints:             node.kubernetes.io/disk-pressure:NoSchedule
Unschedulable:      false
Lease:
  HolderIdentity:  kube01
  AcquireTime:     <unset>
  RenewTime:       Tue, 20 Dec 2022 17:16:39 +0900
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Tue, 20 Dec 2022 14:33:02 +0900   Tue, 20 Dec 2022 14:33:02 +0900   CalicoIsUp                   Calico is running on this node
  MemoryPressure       False   Tue, 20 Dec 2022 17:16:38 +0900   Sun, 27 Nov 2022 17:38:40 +0900   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         True    Tue, 20 Dec 2022 17:16:38 +0900   Tue, 20 Dec 2022 17:04:13 +0900   KubeletHasDiskPressure       kubelet has disk pressure
  PIDPressure          False   Tue, 20 Dec 2022 17:16:38 +0900   Sun, 27 Nov 2022 17:38:40 +0900   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Tue, 20 Dec 2022 17:16:38 +0900   Sun, 27 Nov 2022 17:40:40 +0900   KubeletReady                 kubelet is posting ready status. AppArmor enabled
Addresses:
  InternalIP:  192.168.64.6
  Hostname:    kube01
Capacity:
  cpu:                2
  ephemeral-storage:  9982728Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  hugepages-32Mi:     0
  hugepages-64Ki:     0
  memory:             4004328Ki
  pods:               110
Allocatable:
  cpu:                1800m
  ephemeral-storage:  9200082110
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  hugepages-32Mi:     0
  hugepages-64Ki:     0
  memory:             3377640Ki
  pods:               110
System Info:
  Machine ID:                 c5237e2f98fe4706a98ac34e87b03dd2
  System UUID:                c5237e2f98fe4706a98ac34e87b03dd2
  Boot ID:                    7201145a-5e8e-4853-a166-b776a905fabc
  Kernel Version:             5.15.0-56-generic
  OS Image:                   Ubuntu 22.04.1 LTS
  Operating System:           linux
  Architecture:               arm64
  Container Runtime Version:  containerd://1.6.10
  Kubelet Version:            v1.25.4
  Kube-Proxy Version:         v1.25.4
PodCIDR:                      10.233.65.0/24
PodCIDRs:                     10.233.65.0/24
Non-terminated Pods:          (7 in total)
  Namespace                   Name                              CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                              ------------  ----------  ---------------  -------------  ---
  kube-system                 calico-node-vkvpg                 150m (8%)     300m (16%)  64M (1%)         500M (14%)     22d
  kube-system                 coredns-588bb58b94-xtsgs          100m (5%)     0 (0%)      70Mi (2%)        300Mi (9%)     22d
  kube-system                 kube-apiserver-kube01             250m (13%)    0 (0%)      0 (0%)           0 (0%)         22d
  kube-system                 kube-controller-manager-kube01    200m (11%)    0 (0%)      0 (0%)           0 (0%)         22d
  kube-system                 kube-proxy-nggj9                  0 (0%)        0 (0%)      0 (0%)           0 (0%)         22d
  kube-system                 kube-scheduler-kube01             100m (5%)     0 (0%)      0 (0%)           0 (0%)         22d
  kube-system                 nodelocaldns-bxm48                100m (5%)     0 (0%)      70Mi (2%)        200Mi (6%)     22d
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests        Limits
  --------           --------        ------
  cpu                900m (50%)      300m (16%)
  memory             210800640 (6%)  1024288k (29%)
  ephemeral-storage  0 (0%)          0 (0%)
  hugepages-1Gi      0 (0%)          0 (0%)
  hugepages-2Mi      0 (0%)          0 (0%)
  hugepages-32Mi     0 (0%)          0 (0%)
  hugepages-64Ki     0 (0%)          0 (0%)
Events:
  Type     Reason                Age                   From     Message
  ----     ------                ----                  ----     -------
  Normal   NodeHasDiskPressure   12m                   kubelet  Node kube01 status is now: NodeHasDiskPressure
  Warning  FreeDiskSpaceFailed   10m                   kubelet  failed to garbage collect required amount of images. Wanted to free 2044429926 bytes, but freed 0 bytes
  Warning  EvictionThresholdMet  2m25s (x53 over 12m)  kubelet  Attempting to reclaim ephemeral-storage
```
    - `NodeHasDiskPressure` 이벤트 확인이 가능하다
    - 정리
```shell
 ~/w/study/k8s-24-steps  k delete deployments.apps busybox                                                        ok  kubernetes-admin@cluster.local kube  17:16:40
deployment.apps "busybox" deleted
 ~/w/study/k8s-24-steps  kgpwide                                                                                  ok  kubernetes-admin@cluster.local kube  17:18:48
NAME                      READY   STATUS        RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
busybox-9ff887cc8-487jr   1/1     Terminating   0          23m   10.233.85.206   kube02   <none>           <none>
busybox-9ff887cc8-f7gtp   1/1     Terminating   0          23m   10.233.85.207   kube02   <none>           <none>
busybox-9ff887cc8-fnkcc   1/1     Terminating   0          12m   10.233.85.209   kube02   <none>           <none>
busybox-9ff887cc8-l7fdb   1/1     Terminating   0          23m   10.233.85.205   kube02   <none>           <none>
busybox-9ff887cc8-mv8v4   1/1     Terminating   0          23m   10.233.102.17   kube00   <none>           <none>
busybox-9ff887cc8-nnkrx   1/1     Terminating   0          23m   10.233.102.18   kube00   <none>           <none>
busybox-9ff887cc8-rn2v6   1/1     Terminating   0          13m   10.233.102.21   kube00   <none>           <none>
busybox-9ff887cc8-rtwtm   1/1     Terminating   0          23m   10.233.102.19   kube00   <none>           <none>
busybox-9ff887cc8-sdgld   1/1     Terminating   0          13m   10.233.85.208   kube02   <none>           <none>
busybox-9ff887cc8-swz78   1/1     Terminating   0          14m   10.233.102.20   kube00   <none>           <none>
nginx                     1/1     Running       0          97m   10.233.85.202   kube02   <none>           <none>
nginx-19                  1/1     Running       0          48m   10.233.85.204   kube02   <none>           <none>
 ~/workspace/study/k8s-24-steps  m shell kube01                                                                                                        ok  17:18:56
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-56-generic aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Dec 20 17:19:01 KST 2022

  System load:             1.98583984375
  Usage of /:              99.8% of 9.52GB
  Memory usage:            23%
  Swap usage:              0%
  Processes:               130
  Users logged in:         0
  IPv4 address for enp0s1: 192.168.64.6
  IPv6 address for enp0s1: fdec:f006:592:f10d:5054:ff:fe8d:ef7f

  => / is using 99.8% of 9.52GB

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

9 updates can be applied immediately.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Tue Dec 20 17:05:02 2022 from 192.168.64.1
ubuntu@kube01:~$ ls
2.2g-file  28g-file  snap
ubuntu@kube01:~$ rm *-file
ubuntu@kube01:~$ ll
total 44
drwxr-x--- 6 ubuntu ubuntu 4096 Dec 20 17:19 ./
drwxr-xr-x 3 root   root   4096 Nov 27 17:06 ../
drwx------ 3 ubuntu ubuntu 4096 Nov 27 17:26 .ansible/
-rw------- 1 ubuntu ubuntu  593 Dec 20 17:05 .bash_history
-rw-r--r-- 1 ubuntu ubuntu  220 Jan  7  2022 .bash_logout
-rw-r--r-- 1 ubuntu ubuntu 3771 Jan  7  2022 .bashrc
drwx------ 2 ubuntu ubuntu 4096 Nov 27 17:06 .cache/
-rw-r--r-- 1 ubuntu ubuntu  807 Jan  7  2022 .profile
drwx------ 2 ubuntu ubuntu 4096 Nov 27 17:10 .ssh/
-rw-r--r-- 1 ubuntu ubuntu    0 Nov 27 17:20 .sudo_as_admin_successful
-rw------- 1 ubuntu ubuntu 3432 Nov 27 17:10 .viminfo
drwx------ 3 ubuntu ubuntu 4096 Nov 27 17:24 snap/
ubuntu@kube01:~$ exit
logout
 ~/w/study/k8s-24-steps  kgpwide                                                                             ok  11s  kubernetes-admin@cluster.local kube  17:19:12
NAME       READY   STATUS    RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
nginx      1/1     Running   0          97m   10.233.85.202   kube02   <none>           <none>
nginx-19   1/1     Running   0          49m   10.233.85.204   kube02   <none>           <none>
```
06. 06장: 헬름 기반으로 애플리케이션 설치하기 
  [[../helm]]
  - https://artifacthub.io 단일 헬름 차트 저장소
  - 차트내 `values.yaml` 파일을 통해 변수화 하고 `template` 디렉토리의 yaml 에 변수를 적용하는 방식
  - helm permission warning 제거
```shell
 ~/w/study/k8s-24-steps  helm repo list                                                                        1 err  kubernetes-admin@cluster.local kube  17:31:06
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /Users/deptno/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /Users/deptno/.kube/config
NAME    URL
bitnami https://charts.bitnami.com/bitnami
 ~/workspace/study/k8s-24-steps  ll ~/.kube/config                                                                                                     ok  17:32:07
Permissions Links Size User   Date Modified    Git Name
.rw-r--r--      1 5.7k deptno 2022-12-20 15:54  -I  /Users/deptno/.kube/config
 ~/workspace/study/k8s-24-steps  chmod 600 ~/.kube/config                                                                                              ok  17:32:16
 ~/w/study/k8s-24-steps  helm repo list                                                                           ok  kubernetes-admin@cluster.local kube  17:32:40
NAME    URL
bitnami https://charts.bitnami.com/bitnami
 ~/workspace/study/k8s-24-steps  ll ~/.kube/config                                                                                                     ok  17:32:22
Permissions Links Size User   Date Modified    Git Name
.rw-------      1 5.7k deptno 2022-12-20 15:54  -I  /Users/deptno/.kube/config
```
  - chart 설치
```shell
 ~/w/study/k8s-24-steps  helm repo update                                                                      1 err  kubernetes-admin@cluster.local kube  17:40:36
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈Happy Helming!⎈
 ~/w/study/k8s-24-steps  helm search repo nginx                                                                   ok  kubernetes-admin@cluster.local kube  17:40:41
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION
bitnami/nginx                           13.2.19         1.23.3          NGINX Open Source is a web server that can be a...
bitnami/nginx-ingress-controller        9.3.24          1.6.0           NGINX Ingress Controller is an Ingress controll...
bitnami/nginx-intel                     2.1.13          0.4.9           NGINX Open Source for Intel is a lightweight se...
 ~/w/study/k8s-24-steps  helm pull bitnami/nginx                                                                  ok  kubernetes-admin@cluster.local kube  17:41:15
 ~/workspace/study/k8s-24-steps  ls                                                                                                                    ok  17:41:30
busybox-deploy.yaml  busybox-pod.yaml     nginx-13.2.19.tgz    nginx-error-pod.yaml
 ~/workspace/study/k8s-24-steps  tar xfvz nginx-13.2.19.tgz                                                                                            ok  17:41:33
x nginx/Chart.yaml
x nginx/Chart.lock
x nginx/values.yaml
x nginx/values.schema.json
x nginx/templates/NOTES.txt
# ...
 ~/workspace/study/k8s-24-steps  rm nginx-13.2.19.tgz                                                                                                  ok  17:41:44
 ~/workspace/study/k8s-24-steps  mv nginx nginx.tgz                                                                                                    ok  17:41:58
 ~/workspace/study/k8s-24-steps  ls                                                                                                                    ok  17:42:20
busybox-deploy.yaml  busybox-pod.yaml     nginx-13.2.19        nginx-error-pod.yaml
 ~/workspace/study/k8s-24-steps  cd nginx-13.2.19                                                                                                     INT  17:42:24
 ~/workspace/study/k8s-24-steps/nginx-13.2.19  ls                                                                                                      ok  17:42:32
Chart.lock         Chart.yaml         README.md          charts             templates          values.schema.json values.yaml
 ~/workspace/study/k8s-24-steps/nginx-13.2.19  ls templates                                                                                            ok  17:42:39
NOTES.txt                   extra-list.yaml             ingress.yaml                server-block-configmap.yaml svc.yaml
_helpers.tpl                health-ingress.yaml         pdb.yaml                    serviceaccount.yaml         tls-secrets.yaml
deployment.yaml             hpa.yaml                    prometheusrules.yaml        servicemonitor.yaml
 ~/workspace/study/k8s-24-steps/nginx-13.2.19  cp {,my-}values.yaml                                                                                    ok  17:42:45
 ~/workspace/study/k8s-24-steps/nginx-13.2.19  vi my-}alues.yaml                                                                                       ok  17:42:46
 # replicaCount 를 2로 수정
 ~/workspace/study/k8s-24-steps/nginx-13.2.19  ls                                                                                                      ok  17:47:30
Chart.lock         Chart.yaml         README.md          charts             my-values.yaml     templates          values.schema.json values.yaml
 ~/w/st/k/nginx-13.2.19  k create ns nginx                                                                        ok  kubernetes-admin@cluster.local kube  17:47:32
namespace/nginx created
 ~/w/st/k/nginx-13.2.19  k ns nginx                                                                            1 err  kubernetes-admin@cluster.local kube  17:47:54
Context "kubernetes-admin@cluster.local" modified.
Active namespace is "nginx".
 ~/w/st/k/nginx-13.2.19  helm install nginx -f my-values.yaml .                                          1 err  kubernetes-admin@cluster.local/nginx kube  17:48:54
NAME: nginx
LAST DEPLOYED: Tue Dec 20 17:49:04 2022
NAMESPACE: nginx
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 13.2.19
APP VERSION: 1.23.3

** Please be patient while the chart is being deployed **
NGINX can be accessed through the following DNS name from within your cluster:

    nginx.nginx.svc.cluster.local (port 80)

To access NGINX from outside the cluster, follow the steps below:

1. Get the NGINX URL by running these commands:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace nginx -w nginx'

    export SERVICE_PORT=$(kubectl get --namespace nginx -o jsonpath="{.spec.ports[0].port}" services nginx)
    export SERVICE_IP=$(kubectl get svc --namespace nginx nginx -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
 ~/w/st/k/nginx-13.2.19  helm ls                                                                            ok  kubernetes-admin@cluster.local/nginx kube  17:49:04
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
nginx   nginx           1               2022-12-20 17:49:04.604435 +0900 KST    deployed        nginx-13.2.19   1.23.3
```
    - `CrashLoopBackOff` 트러블 슈팅
```shell
 ~/w/st/k/nginx-13.2.19  kgp                                                                                ok  kubernetes-admin@cluster.local/nginx kube  18:20:44
NAME                     READY   STATUS             RESTARTS        AGE
nginx-6bb994745d-86tvg   0/1     CrashLoopBackOff   7 (2m10s ago)   13m
nginx-6bb994745d-z9h8t   0/1     CrashLoopBackOff   7 (2m27s ago)   13m
 ~/w/st/k/nginx-13.2.19  k logs nginx-6bb994745d-                                                           ok  kubernetes-admin@cluster.local/nginx kube  18:20:59
Error from server (NotFound): pods "nginx-6bb994745d-" not found
 ~/w/st/k/nginx-13.2.19  k logs nginx-6bb994745d-86tvg                                                   1 err  kubernetes-admin@cluster.local/nginx kube  18:21:09
exec /opt/bitnami/scripts/nginx/entrypoint.sh: exec format error
```
        m1 에서 이미지가 실행 되지 않는 것으로 보인다
        - https://github.com/bitnami/bitnami-docker-nginx/issues/178
        - https://github.com/bitnami/charts/issues/7305
        - https://github.com/canonical/multipass/issues/886
        - stress 이미지도 마찬가지로 되지 않는다
    - 모든 파드 정리
```shell
kdelp --all
```
  - resource 보장
    - memory 는 보장받는다 노드의 물리적인 메모리가 부족한 경우 limits 는 보장받지 못한다.
07. 07장: 쿠버네티스 서비스 사용하기 
  - service
    - service 가 label selector 를 가지면 endpoint 가 자동 생성
      - 서비스 생성 후 endpoint 확인하는 습관
    - 내부에서 도메인 이름으로 통신
    - clusterIp 는 cluster 내에서 유효하며 한 node 에 국한되지 않는다
    - nodePort 는 node 가 노출하는 실제 포트로 외부 접속이 가능하다
    - nodePort 는 clusterIp 를 바라보고 각기 다른 노드에 퍼져있는 파드의 endpoint 로 리다이렉트
    - nodePort 는 포트 제약이 있음 30000 ~ 32767
    - nodePort 는 모든 노드에 대해서 포트 개방이 이루어진다
  - endpoint
    - https://i.stack.imgur.com/BGv4C.png 이미지 참조
    - pod 의 ip:port 로 생각하면 된다
  - cluster 내에서는 service 이름으로 통신이 가능
  - 접근 구조
    - loadBalancer 외부 요청
      - nodePort
        - clusterIp
          - endpoint00
          - endpoint01
          - endpoint02
      - nodePort
        - clusterIp
          - endpoint00
          - endpoint01
          - endpoint02
      - nodePort
        - clusterIp
          - endpoint00
          - endpoint01
          - endpoint02
  - dns 
    - coredns 이중화 파드로 동작
      - localdns coredns 의 캐시로 daemonset 으로 동작
    - 요청시 [service].[namespace].svc.cluster.local 형태로 전달되며 같은 namespace 인경우 [service] 만으로 통신이 가능
    - [[../nslookup]]
  - kube-proxy 
    - ipvs 
      - ipvsadm 라우팅 테이블 확인
    - iptables
08. 08장: MetalLB를 이용한 로드밸런서 타입 서비스 구축 
  - metallb 버전이 0.13.x 로 진입하면서 `configInline` 설정이 각 crd 로 변경되었다.
  - 때문에 추가적인 crd 생성이 필요
    - IPAddressPool 사용하지 않는 ip 대역대를 잡아둔다
    - L2Advertisement LoadBalancer 가 생성되면 arp 를 통해서 외부 접속이 가능하도록 한다
  - [[../kubetail]] 여러 파드의 로그 보기
  - [[../k6]] 을 활용한 부하 테스트
  - 단절 확인, 노드 중 어떤 노드를 reboot 해도 단절이 확인 됨
```shell
while true; do curl -I 192.168.64.50 --silent | grep -E 'Date|OK'; sleep 1; done
```
```shell
$ ssh kube02
$$ sudo reboot 
```
09. 09장: Traefik을 이용한 쿠버네티스 인그레스 구축 
  - 설치
```
helm repo add traefik https://traefik.github.io/charts
helm repo update
helm pull traefik
# 수정 후
helm install traefik -f values.yml .
```
  - 별도의 crd 인 ingressroute 를 생성해서 라우팅 한다
  - 기본적으로 lets-encrypt 를 지원한다
  - [ ] tls 설정을 먹이면 http 접속이 먹통이 된다
  - 대시보드도 제공된다 9000 포트
  - 자체 인증서를 제공할 수도 있다. **생략**


24. 24장: 쿠버네티스 노드 변경과 추가 
  - ubuntu server 설치 + openssh
```shell
vi /etc/hosts # 노드 정보 추가
vi inventory/mycluster/hosts.yml # host 정보 추가 + node 추가
ansible-playbook -i inventory/mycluster/hosts.yml -b facts.yml
ansible-playbook -i inventory/mycluster/hosts.yml -b scale.yml --limit=[nodename]
```

## architecture issue
```shell
exec /opt/bitnami/scripts/nginx/entrypoint.sh: exec format error
```
실습에 사용된 특정 image 들은 arm 에서 실행이 불가능하다.  
x86 emulation 이 필요하다.

## related
- [[../kubernetes|kubernetes]]
- [[../kubespray]]
