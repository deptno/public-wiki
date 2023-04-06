# traefik

v2.9

- traefik container 가 api 권한을 가져야하므로 ServiceAccount 가 필요
  + https://doc.traefik.io/traefik/getting-started/quick-start-with-kubernetes/
- 해당 권한으로 traefik deployment 가 생성되며 여기서 포트와 함께 dashboard도 함께 처리
- options
  - static 
    - entry point
      - port 
      - protocol
    - provider
      - infrastructure component(container engine, cloud provider) - api server 를 사용하기 때문에 연관성이 있음
    - connection
    - information
  - dynamic
  - 옵션 적용 순서
    1. config file
      - /etc/traefik/
      - $XDG_CONFIG_HOME/
      - $HOME/.config/
    2. cli options
    3. 환경 변수
- installation
  - helm 으로 설치시 dashboard 까지 모두 설치됨
```
kubectl port-forward $(kubectl get pods --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000
```
  - http://localhost:9000/dashboard/ **tail slash 가 필수다**
- faq
  - 404 라우터 매칭 X
  - 502 라우터는 매칭 O, 서비스 없음
  - 503 라우터는 매칭 O, 서버 없음
  - catchall 로 statuscode 변경 가능 + [[cloudfront]] 와 연동등에 유용
  - http to https 옵션은 2.9.9 helm chart 를 기준으로 `redirectTo: https` 을 활성화 하면 된다
- reload
  - file watch 이므로 컨피그가 변경되면 적용되나 컨피그에서 참조중인 tls 등 인증서 참조는 자동으로 변경되지 않는다
  - `touch` 사용하자
- tls
  - [[let's-encrypt]] 를 사용하면서 [[ha]] 를 달성할 수 없다
    - enterprise 사용으로 달성 가능
    - cert-manager 로 달성 가능
    
## tls

tls 는 잘안되다가 자고 일어나니 되는 경향이 있어서 시간을 좀 가질 필요도 있다. 인증서 발급에 걸리는 시간으로 보임

+ https://doc.traefik.io/traefik/https/acme/#providers
+ https://doc.traefik.io/traefik/user-guides/crd-acme/
  - default 8000/8443
```
  ports:
    - protocol: TCP
      name: web
      port: 8000
    - protocol: TCP
      name: admin
      port: 8080
    - protocol: TCP
      name: websecure
      port: 4443
```
+ https://traefik.io/blog/install-and-configure-traefik-with-helm/

### [[synology]] nas
dsm 7 기준
제어판 -> 로그인 포털 -> `자동으로 HTTP 연결을 DSM 데스크톱의 HTTPS 로 리디렉션`
을 해제하고 headless service 의 80 -> 5000(DSM 데스크톱 기본 포트) 로 연결하여 새로 발행한 인증서로 사용해야한다.

## error
```sh 
 tls: client offered only unsupported versions: [301]
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="Serving default certificate for request: \"[domain.name]\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="http: TLS handshake error from 192.168.0.7:20798: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="http: TLS handshake error from 192.168.0.7:52461: tls: client offered only unsupported versions: [301]"
```

```sh
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:45:21Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:07Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:07Z" level=debug msg="http: TLS handshake error from 192.168.0.7:58054: remote error: tls: unknown certificate authority"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="http: TLS handshake error from 192.168.0.7:6517: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="http: TLS handshake error from 192.168.0.7:18767: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="http: TLS handshake error from 192.168.0.7:60383: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="http: TLS handshake error from 192.168.0.7:24009: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="http: TLS handshake error from 192.168.0.7:35808: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:15Z" level=debug msg="http: TLS handshake error from 192.168.0.7:49872: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="http: TLS handshake error from 192.168.0.7:14679: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="http: TLS handshake error from 192.168.0.7:25925: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="http: TLS handshake error from 192.168.0.7:34121: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="http: TLS handshake error from 192.168.0.7:62899: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="http: TLS handshake error from 192.168.0.7:20798: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:49:24Z" level=debug msg="http: TLS handshake error from 192.168.0.7:52461: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:50:30Z" level=debug msg="Serving default certificate for request: \"\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:50:38Z" level=debug msg="Serving default certificate for request: \"\""
k[traefik-5b88b748d-ddhp5] time="2023-01-14T06:52:25Z" level=debug msg="Serving default certificate for request: \"\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="http: TLS handshake error from 192.168.0.7:9941: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="http: TLS handshake error from 192.168.0.7:11811: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="http: TLS handshake error from 192.168.0.7:37216: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="http: TLS handshake error from 192.168.0.7:16574: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="http: TLS handshake error from 192.168.0.7:35759: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:53:20Z" level=debug msg="http: TLS handshake error from 192.168.0.7:42656: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:57:04Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:57:04Z" level=debug msg="http: TLS handshake error from 192.168.0.7:11717: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:57:04Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:57:04Z" level=debug msg="http: TLS handshake error from 192.168.0.7:60905: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:57:04Z" level=debug msg="http: TLS handshake error from 192.168.0.7:41380: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:58:26Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:58:26Z" level=debug msg="http: TLS handshake error from 192.168.0.7:13340: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:58:26Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:58:26Z" level=debug msg="http: TLS handshake error from 192.168.0.7:12410: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:58:26Z" level=debug msg="http: TLS handshake error from 192.168.0.7:43490: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:05Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:05Z" level=debug msg="http: TLS handshake error from 192.168.0.7:64332: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:05Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:05Z" level=debug msg="http: TLS handshake error from 192.168.0.7:6436: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:05Z" level=debug msg="http: TLS handshake error from 192.168.0.7:38429: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:28338: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:45046: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:12799: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:33325: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:11867: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:53477: tls: client offered only unsupported versions: [301]"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:4607: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="Serving default certificate for request: \"cluster.deptno.dev\""
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:10706: EOF"
[traefik-5b88b748d-ddhp5] time="2023-01-14T06:59:16Z" level=debug msg="http: TLS handshake error from 192.168.0.7:25606: tls: client offered only unsupported versions: [301]"
^R
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="No secret name provided" providerName=kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Configuration received: {\"http\":{\"routers\":{\"default-traefik-dashboard-d012b7f875133eeab4e5\":{\"entryPoints\":[\"traefik\"],\"service\":\"api@internal\",\"rule\":\"PathPrefix(`/dashboard`) || PathPrefix(`/api`)\"},\"traefik-service-route-9ab4060701404e59ffcd\":{\"entryPoints\":[\"web\",\"websecure\"],\"service\":\"traefik-service-route-9ab4060701404e59ffcd\",\"rule\":\"PathPrefix(`/whoami`)\",\"tls\":{\"certResolver\":\"letsencrypt\"}},\"traefik-service-route-e663a23b674cedfd3387\":{\"entryPoints\":[\"web\",\"websecure\"],\"service\":\"traefik-service-route-e663a23b674cedfd3387\",\"rule\":\"Host(`cluster.deptno.dev`)\",\"tls\":{\"certResolver\":\"letsencrypt\"}}},\"services\":{\"traefik-service-route-9ab4060701404e59ffcd\":{\"loadBalancer\":{\"servers\":[{\"url\":\"http://10.244.182.135:80\"}],\"passHostHeader\":true}},\"traefik-service-route-e663a23b674cedfd3387\":{\"loadBalancer\":{\"servers\":[{\"url\":\"http://10.244.182.135:80\"}],\"passHostHeader\":true}}}},\"tcp\":{},\"udp\":{},\"tls\":{}}" providerName=kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="No default certificate, fallback to the internal generated certificate" tlsStoreName=default
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Added outgoing tracing middleware prometheus@internal" middlewareType=TracingForwarder middlewareName=tracing entryPointName=metrics routerName=prometheus@internal
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=metrics middlewareName=traefik-internal-recovery middlewareType=Recovery
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Added outgoing tracing middleware api@internal" routerName=default-traefik-dashboard-d012b7f875133eeab4e5@kubernetescrd middlewareName=tracing middlewareType=TracingForwarder entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Added outgoing tracing middleware ping@internal" entryPointName=traefik middlewareName=tracing middlewareType=TracingForwarder routerName=ping@internal
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=traefik middlewareName=traefik-internal-recovery middlewareType=Recovery
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Added outgoing tracing middleware acme-http@internal" routerName=acme-http@internal middlewareName=tracing middlewareType=TracingForwarder entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" middlewareName=traefik-internal-recovery middlewareType=Recovery entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=metrics middlewareType=Metrics middlewareName=metrics-entrypoint
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" middlewareName=metrics-entrypoint middlewareType=Metrics entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" middlewareType=Metrics middlewareName=metrics-entrypoint entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" middlewareType=Metrics entryPointName=websecure middlewareName=metrics-entrypoint
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" serviceName=traefik-service-route-e663a23b674cedfd3387 middlewareName=pipelining middlewareType=Pipelining entryPointName=web routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=web routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 middlewareName=metrics-service middlewareType=Metrics
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating load-balancer" routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating server 0 http://10.244.182.135:80" entryPointName=web routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd serverName=0 serviceName=traefik-service-route-e663a23b674cedfd3387
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="child http://10.244.182.135:80 now UP"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Propagating new UP status"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Added outgoing tracing middleware traefik-service-route-e663a23b674cedfd3387" routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd middlewareName=tracing middlewareType=TracingForwarder entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=web routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd middlewareName=pipelining middlewareType=Pipelining
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd middlewareName=metrics-service middlewareType=Metrics entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating load-balancer" entryPointName=web routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating server 0 http://10.244.182.135:80" serviceName=traefik-service-route-9ab4060701404e59ffcd entryPointName=web routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serverName=0
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="child http://10.244.182.135:80 now UP"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Propagating new UP status"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Added outgoing tracing middleware traefik-service-route-9ab4060701404e59ffcd" middlewareName=tracing middlewareType=TracingForwarder entryPointName=web routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=web middlewareName=traefik-internal-recovery middlewareType=Recovery
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 middlewareName=pipelining middlewareType=Pipelining entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=websecure middlewareName=metrics-service middlewareType=Metrics routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating load-balancer" serviceName=traefik-service-route-e663a23b674cedfd3387 entryPointName=websecure routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating server 0 http://10.244.182.135:80" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 serverName=0 entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="child http://10.244.182.135:80 now UP"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Propagating new UP status"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Added outgoing tracing middleware traefik-service-route-e663a23b674cedfd3387" middlewareName=tracing middlewareType=TracingForwarder routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" serviceName=traefik-service-route-9ab4060701404e59ffcd middlewareName=pipelining middlewareType=Pipelining entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd middlewareName=metrics-service middlewareType=Metrics
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating load-balancer" entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating server 0 http://10.244.182.135:80" serviceName=traefik-service-route-9ab4060701404e59ffcd entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serverName=0
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="child http://10.244.182.135:80 now UP"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Propagating new UP status"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Added outgoing tracing middleware traefik-service-route-9ab4060701404e59ffcd" routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd middlewareName=tracing middlewareType=TracingForwarder entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" entryPointName=websecure middlewareName=traefik-internal-recovery middlewareType=Recovery
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" middlewareType=Metrics entryPointName=metrics middlewareName=metrics-entrypoint
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" middlewareName=metrics-entrypoint middlewareType=Metrics entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" middlewareName=metrics-entrypoint middlewareType=Metrics entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Creating middleware" middlewareType=Metrics entryPointName=websecure middlewareName=metrics-entrypoint
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=warning msg="No domain found in rule PathPrefix(`/whoami`), the TLS options applied for this router will depend on the SNI of each request" entryPointName=web routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Adding route for cluster.deptno.dev with TLS options default" entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=warning msg="No domain found in rule PathPrefix(`/whoami`), the TLS options applied for this router will depend on the SNI of each request" routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Adding route for cluster.deptno.dev with TLS options default" entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="No domain parsed in provider ACME" ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd rule="PathPrefix(`/whoami`)" providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="No domain parsed in provider ACME" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd rule="PathPrefix(`/whoami`)"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Trying to challenge certificate for domain [cluster.deptno.dev] found in HostSNI rule" routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Trying to challenge certificate for domain [cluster.deptno.dev] found in HostSNI rule" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Looking for provided certificate(s) to validate [\"cluster.deptno.dev\"]..." routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Domains [\"cluster.deptno.dev\"] need ACME certificates generation for domains \"cluster.deptno.dev\"." rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Loading ACME certificates [cluster.deptno.dev]..." routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Looking for provided certificate(s) to validate [\"cluster.deptno.dev\"]..." routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="No ACME certificate generation required for domains [\"cluster.deptno.dev\"]." routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Building ACME client..." providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="https://acme-v02.api.letsencrypt.org/directory" providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="No secret name provided" providerName=kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=debug msg="Using DNS Challenge provider: digitalocean" providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:07Z" level=error msg="Unable to obtain ACME certificate for domains \"cluster.deptno.dev\": cannot get ACME client digitalocean: some credentials information are missing: DO_AUTH_TOKEN" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Configuration received: {\"http\":{\"routers\":{\"default-traefik-dashboard-d012b7f875133eeab4e5\":{\"entryPoints\":[\"traefik\"],\"service\":\"api@internal\",\"rule\":\"PathPrefix(`/dashboard`) || PathPrefix(`/api`)\"},\"traefik-service-route-9ab4060701404e59ffcd\":{\"entryPoints\":[\"web\",\"websecure\"],\"service\":\"traefik-service-route-9ab4060701404e59ffcd\",\"rule\":\"PathPrefix(`/whoami`)\",\"tls\":{\"certResolver\":\"letsencrypt\"}},\"traefik-service-route-e663a23b674cedfd3387\":{\"entryPoints\":[\"web\",\"websecure\"],\"service\":\"traefik-service-route-e663a23b674cedfd3387\",\"rule\":\"Host(`cluster.deptno.dev`)\",\"tls\":{\"certResolver\":\"letsencrypt\"}},\"traefik-traefik-dashboard-d012b7f875133eeab4e5\":{\"entryPoints\":[\"traefik\"],\"service\":\"api@internal\",\"rule\":\"PathPrefix(`/dashboard`) || PathPrefix(`/api`)\"}},\"services\":{\"traefik-service-route-9ab4060701404e59ffcd\":{\"loadBalancer\":{\"servers\":[{\"url\":\"http://10.244.182.135:80\"}],\"passHostHeader\":true}},\"traefik-service-route-e663a23b674cedfd3387\":{\"loadBalancer\":{\"servers\":[{\"url\":\"http://10.244.182.135:80\"}],\"passHostHeader\":true}}}},\"tcp\":{},\"udp\":{},\"tls\":{}}" providerName=kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="No default certificate, fallback to the internal generated certificate" tlsStoreName=default
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware prometheus@internal" entryPointName=metrics routerName=prometheus@internal middlewareName=tracing middlewareType=TracingForwarder
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" entryPointName=metrics middlewareName=traefik-internal-recovery middlewareType=Recovery
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware acme-http@internal" routerName=acme-http@internal middlewareName=tracing middlewareType=TracingForwarder entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareType=Recovery entryPointName=web middlewareName=traefik-internal-recovery
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware api@internal" routerName=default-traefik-dashboard-d012b7f875133eeab4e5@kubernetescrd middlewareName=tracing middlewareType=TracingForwarder entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware ping@internal" routerName=ping@internal entryPointName=traefik middlewareName=tracing middlewareType=TracingForwarder
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware api@internal" entryPointName=traefik routerName=traefik-traefik-dashboard-d012b7f875133eeab4e5@kubernetescrd middlewareName=tracing middlewareType=TracingForwarder
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" entryPointName=traefik middlewareName=traefik-internal-recovery middlewareType=Recovery
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" entryPointName=metrics middlewareName=metrics-entrypoint middlewareType=Metrics
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareType=Metrics entryPointName=traefik middlewareName=metrics-entrypoint
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareName=metrics-entrypoint middlewareType=Metrics entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareName=metrics-entrypoint middlewareType=Metrics entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareName=pipelining entryPointName=websecure routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 middlewareType=Pipelining
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" serviceName=traefik-service-route-e663a23b674cedfd3387 middlewareName=metrics-service middlewareType=Metrics entryPointName=websecure routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating load-balancer" serviceName=traefik-service-route-e663a23b674cedfd3387 entryPointName=websecure routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating server 0 http://10.244.182.135:80" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 serverName=0 entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="child http://10.244.182.135:80 now UP"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Propagating new UP status"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware traefik-service-route-e663a23b674cedfd3387" entryPointName=websecure routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd middlewareName=tracing middlewareType=TracingForwarder
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareType=Pipelining entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd middlewareName=pipelining
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareName=metrics-service middlewareType=Metrics entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating load-balancer" serviceName=traefik-service-route-9ab4060701404e59ffcd entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating server 0 http://10.244.182.135:80" routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serverName=0 serviceName=traefik-service-route-9ab4060701404e59ffcd entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="child http://10.244.182.135:80 now UP"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Propagating new UP status"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware traefik-service-route-9ab4060701404e59ffcd" entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd middlewareName=tracing middlewareType=TracingForwarder
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareName=traefik-internal-recovery middlewareType=Recovery entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareType=Pipelining routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 entryPointName=web middlewareName=pipelining
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareType=Metrics routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 entryPointName=web middlewareName=metrics-service
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating load-balancer" entryPointName=web routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating server 0 http://10.244.182.135:80" serverName=0 routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd serviceName=traefik-service-route-e663a23b674cedfd3387 entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="child http://10.244.182.135:80 now UP"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Propagating new UP status"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware traefik-service-route-e663a23b674cedfd3387" middlewareName=tracing middlewareType=TracingForwarder entryPointName=web routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd middlewareName=pipelining middlewareType=Pipelining entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareType=Metrics entryPointName=web routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd middlewareName=metrics-service
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating load-balancer" routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating server 0 http://10.244.182.135:80" routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd serviceName=traefik-service-route-9ab4060701404e59ffcd entryPointName=web serverName=0
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="child http://10.244.182.135:80 now UP"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Propagating new UP status"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Added outgoing tracing middleware traefik-service-route-9ab4060701404e59ffcd" routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd middlewareName=tracing middlewareType=TracingForwarder entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareName=traefik-internal-recovery middlewareType=Recovery entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareType=Metrics entryPointName=metrics middlewareName=metrics-entrypoint
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareName=metrics-entrypoint middlewareType=Metrics entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" middlewareName=metrics-entrypoint middlewareType=Metrics entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Creating middleware" entryPointName=websecure middlewareType=Metrics middlewareName=metrics-entrypoint
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=warning msg="No domain found in rule PathPrefix(`/whoami`), the TLS options applied for this router will depend on the SNI of each request" routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Adding route for cluster.deptno.dev with TLS options default" entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=warning msg="No domain found in rule PathPrefix(`/whoami`), the TLS options applied for this router will depend on the SNI of each request" entryPointName=websecure routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Adding route for cluster.deptno.dev with TLS options default" entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="No domain parsed in provider ACME" ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=traefik-service-route-9ab4060701404e59ffcd@kubernetescrd rule="PathPrefix(`/whoami`)" providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="No domain parsed in provider ACME" routerName=websecure-traefik-service-route-9ab4060701404e59ffcd@kubernetescrd rule="PathPrefix(`/whoami`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Trying to challenge certificate for domain [cluster.deptno.dev] found in HostSNI rule" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Trying to challenge certificate for domain [cluster.deptno.dev] found in HostSNI rule" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Looking for provided certificate(s) to validate [\"cluster.deptno.dev\"]..." ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Domains [\"cluster.deptno.dev\"] need ACME certificates generation for domains \"cluster.deptno.dev\"." providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Loading ACME certificates [cluster.deptno.dev]..." rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Building ACME client..." providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="https://acme-v02.api.letsencrypt.org/directory" providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Looking for provided certificate(s) to validate [\"cluster.deptno.dev\"]..." ACME CA="https://acme-v02.api.letsencrypt.org/directory" routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="No ACME certificate generation required for domains [\"cluster.deptno.dev\"]." routerName=traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="No secret name provided" providerName=kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Skipping Kubernetes event kind *v1.Endpoints" providerName=kubernetes
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Skipping Kubernetes event kind *v1.Endpoints" providerName=kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="No secret name provided" providerName=kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Skipping Kubernetes event kind *v1.Endpoints" providerName=kubernetes
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Skipping Kubernetes event kind *v1.Endpoints" providerName=kubernetescrd
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=info msg="I have to go..."
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=info msg="Stopping server gracefully"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Waiting 10s seconds before killing connections." entryPointName=metrics
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Waiting 10s seconds before killing connections." entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Waiting 10s seconds before killing connections." entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="accept tcp [::]:9100: use of closed network connection" entryPointName=metrics
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Waiting 10s seconds before killing connections." entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="accept tcp [::]:8443: use of closed network connection" entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="close tcp [::]:9100: use of closed network connection" entryPointName=metrics
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Entry point metrics closed" entryPointName=metrics
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="accept tcp [::]:8000: use of closed network connection" entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="accept tcp [::]:9000: use of closed network connection" entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="close tcp [::]:9000: use of closed network connection" entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="close tcp [::]:8000: use of closed network connection" entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Entry point traefik closed" entryPointName=traefik
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Entry point web closed" entryPointName=web
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="close tcp [::]:8443: use of closed network connection" entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Entry point websecure closed" entryPointName=websecure
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=info msg="Server stopped"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=info msg="Shutting down"
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=debug msg="Using DNS Challenge provider: digitalocean" providerName=letsencrypt.acme
[traefik-5b88b748d-ddhp5] time="2023-01-14T07:06:09Z" level=error msg="Unable to obtain ACME certificate for domains \"cluster.deptno.dev\": cannot get ACME client digitalocean: some credentials information are missing: DO_AUTH_TOKEN" routerName=websecure-traefik-service-route-e663a23b674cedfd3387@kubernetescrd rule="Host(`cluster.deptno.dev`)" providerName=letsencrypt.acme ACME CA="https://acme-v02.api.letsencrypt.org/directory"
```

최종적으로 아래와 같은 traefik deployment 를 만들어야함
```yaml
  Containers:
   traefik:
    Image:       traefik:v2.9.6
    Ports:       9100/TCP, 9000/TCP, 8000/TCP, 8443/TCP
    Host Ports:  0/TCP, 0/TCP, 0/TCP, 0/TCP
    Args:
      --global.checknewversion
      --entrypoints.metrics.address=:9100/tcp
      --entrypoints.traefik.address=:9000/tcp
      --entrypoints.web.address=:8000/tcp
      --entrypoints.websecure.address=:8443/tcp
      --api.dashboard=true
      --ping=true
      --metrics.prometheus=true
      --metrics.prometheus.entrypoint=metrics
      --providers.kubernetescrd
      --providers.kubernetesingress
      --entrypoints.websecure.http.tls=true
      --certificatesresolvers.letsencrypt.acme.email=deptno@gmail.com
      --certificatesresolvers.letsencrypt.acme.storage=/data/acme.json
      --certificatesresolvers.letsencrypt.acme.tlsChallenge=true
      --log.level=DEBUG
```
해당 에러가 발생할 때 helm chart 에서 certResolver.letsencrypt.dnsChallenge 를 주석 처리하고 위와 같은 Args 를 만들어야하며
다른 부분 보다는 아래설정이 중요할 것 같다
```yaml
      --entrypoints.web.address=:8000/tcp
      --entrypoints.websecure.address=:8443/tcp
      --entrypoints.websecure.http.tls=true
      --certificatesresolvers.letsencrypt.acme.email=deptno@gmail.com
      --certificatesresolvers.letsencrypt.acme.storage=/data/acme.json
      --certificatesresolvers.letsencrypt.acme.tlsChallenge=true
```
ingress 를 수정해서 tls 프로세스를 밟 을 수 있도록 하면 접근이 가능하다

## CRD: IngressRoute 
IngressRoute 는 참조할 service 가 있는 영역에 생성한다.

## error
`LOG_LEVEL=trace` 를 환경변수로 주입해서 middleware 를 띄우고 로그를 확인
### Error calling http
- traefik pod log
```sh
time="2023-01-21T17:28:46Z" level=debug msg="Error calling http://forward-auth-google. Cause: Get \"http://forward-auth-google
\": dial tcp: lookup forward-auth-google on 10.96.0.10:53: no such host" middlewareType=ForwardedAuthType middlewareName=test-
forward-auth-google@kubernetescrd
```
- middleware pod 에 로그가 안찍히는 경우
  traefik -> middleware 접근이 안되는 경우로 traefik, middleware 가 각기 다른 namespace에 존재할 때 발생
  - traefik 에서 crossname 를 허용
  - [X] forward-auth-google.[namespace] 를 통해서 접근하도록 설정
- 로그인 후 서비스로 가지못하고 계속 로그인으로 리다이렉팅 되는 이슈
  - 이미지 교체 `2.2.0` -> `latest`
    + https://github.com/thomseddon/traefik-forward-auth/issues/198#issuecomment-902513612
### Authenticating request
```sh 
time="2023-01-21T16:43:54Z" level=debug msg="Authenticating request" cookies="[]" handler=Auth host= method= proto= rule=defau
lt source_ip= uri=
time="2023-01-21T16:43:52Z" level=debug msg="Set CSRF cookie and redirected to provider login url" csrf_cookie="_forward_auth_
csrf=e020b6a2d282deed96185016aea24fcf; Path=/; Expires=Sun, 22 Jan 2023 04:43:52 GMT; HttpOnly" handler=Auth host= login_url="
```
- `insecure_cookie=true` 제거로 해결되는 것으로 보임
---
```text
400 오류: redirect_uri_mismatch
```
secret 에서 개행 제거하고 나서 나오기 시작
---
middleware 의 namespace 와 관계없이 ingressroute 의 namespace 를 보는 것 같다
middleware.forwardAuth 를 설정할때 namespace 를 명시한다
## related
- [[kubernetes]]
- [[diary:2023-01-08]]
- [[ingress]]
- [[let's-encrypt]]
- [[service]]
- [[synology]]
- [[forward-auth]]
