# calico

## error
helm3 로 설치 후 모든 파드가 정상적으로 떳으나 아래와 같은 로그가 올라옴

```sh kl kube-apivserver-pi0
W0107 04:27:12.220153       1 handler_proxy.go:106] no RequestInfo found in the context
W0107 04:27:12.220228       1 handler_proxy.go:106] no RequestInfo found in the context
E0107 04:27:12.220352       1 controller.go:113] loading OpenAPI spec for "v3.projectcalico.org" failed with: Error, could not get list of group versions for APIService
I0107 04:27:12.220411       1 controller.go:126] OpenAPI AggregationController: action for item v3.projectcalico.org: Rate Limited Requeue.
E0107 04:27:12.220358       1 controller.go:116] loading OpenAPI spec for "v3.projectcalico.org" failed with: failed to retrieve openAPI spec, http error: ResponseCode: 503, Body: service unavailable
, Header: map[Content-Type:[text/plain; charset=utf-8] X-Content-Type-Options:[nosniff]]
I0107 04:27:12.221820       1 controller.go:129] OpenAPI AggregationController: action for item v3.projectcalico.org: Rate Limited Requeue.
E0107 04:27:16.222155       1 available_controller.go:527] v3.projectcalico.org failed with: failing or missing response from https://10.104.159.195:443/apis/projectcalico.org/v3: Get "https://10.104.159.195:443/apis/projectcalico.org/v3": context deadline exceeded
I0107 04:27:30.541265       1 trace.go:219] Trace[1817643909]: "Get" accept:application/json, */*,audit-id:4863d2b3-5c0d-4cd8-8a6f-cc677eb4e8e9,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/kube-apiserver-pi0/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernet
es/$Format,verb:GET (07-Jan-2023 04:24:00.511) (total time: 210030ms):
Trace[1817643909]: ---"Writing http response done" 210020ms (04:27:30.541)
Trace[1817643909]: [3m30.030029978s] [3m30.030029978s] END
E0107 04:27:36.214371       1 available_controller.go:527] v3.projectcalico.org failed with: failing or missing response from https://10.104.159.195:443/apis/projectcalico.org/v3: Get "https://10.104.159.195:443/apis/projectcalico.org/v3": net/http: request canceled while waiting for connection (Client.Timeout exceed
ed while awaiting headers)
W0107 04:27:37.222793       1 handler_proxy.go:106] no RequestInfo found in the context
W0107 04:27:37.222794       1 handler_proxy.go:106] no RequestInfo found in the context
E0107 04:27:37.222942       1 controller.go:113] loading OpenAPI spec for "v3.projectcalico.org" failed with: Error, could not get list of group versions for APIService
E0107 04:27:37.223129       1 controller.go:116] loading OpenAPI spec for "v3.projectcalico.org" failed with: failed to retrieve openAPI spec, http error: ResponseCode: 503, Body: service unavailable
, Header: map[Content-Type:[text/plain; charset=utf-8] X-Content-Type-Options:[nosniff]]
I0107 04:27:37.223512       1 controller.go:126] OpenAPI AggregationController: action for item v3.projectcalico.org: Rate Limited Requeue.
I0107 04:27:37.224498       1 controller.go:129] OpenAPI AggregationController: action for item v3.projectcalico.org: Rate Limited Requeue.
E0107 04:27:41.217088       1 available_controller.go:527] v3.projectcalico.org failed with: failing or missing response from https://10.104.159.195:443/apis/projectcalico.org/v3: Get "https://10.104.159.195:443/apis/projectcalico.org/v3": context deadline exceeded
I0107 04:27:43.846861       1 trace.go:219] Trace[1595406893]: "Get" accept:application/json, */*,audit-id:6fcae69a-1d5e-4902-8f23-5b9d7b23b495,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/kube-controller-manager-pi0/log,user-agent:k9s/v0.0.0 (darwin/arm64)
 kubernetes/$Format,verb:GET (07-Jan-2023 04:27:33.104) (total time: 10742ms):
Trace[1595406893]: ---"Writing http response done" 10735ms (04:27:43.846)
Trace[1595406893]: [10.742592724s] [10.742592724s] END
E0107 04:27:46.221971       1 available_controller.go:527] v3.projectcalico.org failed with: failing or missing response from https://10.104.159.195:443/apis/projectcalico.org/v3: Get "https://10.104.159.195:443/apis/projectcalico.org/v3": net/http: request canceled while waiting for connection (Client.Timeout exceed
ed while awaiting headers)
I0107 04:27:47.075155       1 trace.go:219] Trace[1049878265]: "Get" accept:application/json, */*,audit-id:3b6d6b1b-3b30-4bd8-8449-d33a27479480,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/kube-proxy-7mgj9/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernetes
/$Format,verb:GET (07-Jan-2023 04:27:44.665) (total time: 2409ms):
Trace[1049878265]: ---"Writing http response done" 2401ms (04:27:47.074)
Trace[1049878265]: [2.409498934s] [2.409498934s] END
I0107 04:27:48.148190       1 trace.go:219] Trace[1806312569]: "Get" accept:application/json, */*,audit-id:0250b743-2c50-4bf5-88d5-adc0776abe7c,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/kube-proxy-knl9w/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernetes
/$Format,verb:GET (07-Jan-2023 04:27:47.541) (total time: 606ms):
Trace[1806312569]: ---"Writing http response done" 598ms (04:27:48.148)
Trace[1806312569]: [606.244355ms] [606.244355ms] END
I0107 04:27:49.836511       1 trace.go:219] Trace[1530363256]: "Get" accept:application/json, */*,audit-id:f4d1ed67-86a9-44a5-bae1-a329539ad625,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/kube-proxy-p7fhv/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernetes
/$Format,verb:GET (07-Jan-2023 04:27:48.658) (total time: 1177ms):
Trace[1530363256]: ---"Writing http response done" 1169ms (04:27:49.836)
Trace[1530363256]: [1.177869134s] [1.177869134s] END
I0107 04:27:51.546063       1 trace.go:219] Trace[363723798]: "Get" accept:application/json, */*,audit-id:d54e6310-9c7b-490c-87eb-dc0a0c5be83b,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/kube-scheduler-pi0/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernete
s/$Format,verb:GET (07-Jan-2023 04:27:50.646) (total time: 899ms):
Trace[363723798]: ---"Writing http response done" 890ms (04:27:51.545)
Trace[363723798]: [899.421768ms] [899.421768ms] END
I0107 04:27:53.926594       1 trace.go:219] Trace[651905021]: "Get" accept:application/json, */*,audit-id:b08605fa-16c8-4fbe-a39c-dc1b45af3f35,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/test/pods/pingtest-8547ccd6f-6smng/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernetes
/$Format,verb:GET (07-Jan-2023 04:27:53.292) (total time: 634ms):
Trace[651905021]: ---"Writing http response done" 627ms (04:27:53.926)
Trace[651905021]: [634.302099ms] [634.302099ms] END
I0107 04:27:56.442154       1 trace.go:219] Trace[1137123119]: "Get" accept:application/json, */*,audit-id:3f0ba5cc-65af-4e6c-b5fb-369119b510a6,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/test/pods/pingtest-8547ccd6f-nsf25/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernete
s/$Format,verb:GET (07-Jan-2023 04:27:55.474) (total time: 967ms):
Trace[1137123119]: ---"Writing http response done" 955ms (04:27:56.441)
Trace[1137123119]: [967.036248ms] [967.036248ms] END
I0107 04:27:57.890463       1 trace.go:219] Trace[969521398]: "Get" accept:application/json, */*,audit-id:438c0297-2465-4b8d-b712-be835fce44b3,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/tigera-operator/pods/tigera-operator-7795f5d79b-js6lp/log,user-agent:k9s/v0.0.0 (darwi
n/arm64) kubernetes/$Format,verb:GET (07-Jan-2023 04:27:57.062) (total time: 828ms):
Trace[969521398]: ---"Writing http response done" 822ms (04:27:57.890)
Trace[969521398]: [828.135684ms] [828.135684ms] END
I0107 04:28:01.670961       1 trace.go:219] Trace[1870349549]: "Get" accept:application/json, */*,audit-id:8ef43547-67ca-4642-90c3-856ba0b572e9,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/coredns-787d4945fb-ngrmj/log,user-agent:k9s/v0.0.0 (darwin/arm64) ku
bernetes/$Format,verb:GET (07-Jan-2023 04:28:00.335) (total time: 1335ms):
Trace[1870349549]: ---"Writing http response done" 1327ms (04:28:01.670)
Trace[1870349549]: [1.335446006s] [1.335446006s] END
E0107 04:28:06.196121       1 available_controller.go:527] v3.projectcalico.org failed with: failing or missing response from https://10.104.159.195:443/apis/projectcalico.org/v3: Get "https://10.104.159.195:443/apis/projectcalico.org/v3": net/http: request canceled while waiting for connection (Client.Timeout exceed
ed while awaiting headers)
I0107 04:28:09.034029       1 trace.go:219] Trace[1055865248]: "Get" accept:application/json, */*,audit-id:7c826fea-601b-4ccd-aa38-8e1381b28178,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/calicoctl/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernetes/$Forma
t,verb:GET (07-Jan-2023 04:28:08.525) (total time: 508ms):
Trace[1055865248]: ---"Writing http response done" 500ms (04:28:09.033)
Trace[1055865248]: [508.219294ms] [508.219294ms] END
E0107 04:28:11.216744       1 available_controller.go:527] v3.projectcalico.org failed with: failing or missing response from https://10.104.159.195:443/apis/projectcalico.org/v3: Get "https://10.104.159.195:443/apis/projectcalico.org/v3": net/http: request canceled while waiting for connection (Client.Timeout exceed
ed while awaiting headers)
I0107 04:28:11.322799       1 trace.go:219] Trace[1434669929]: "Get" accept:application/json, */*,audit-id:23efb644-ada4-4b7a-ba42-68ee8473c431,client:192.168.0.1,protocol:HTTP/2.0,resource:pods,scope:resource,url:/api/v1/namespaces/kube-system/pods/etcd-pi0/log,user-agent:k9s/v0.0.0 (darwin/arm64) kubernetes/$Format
,verb:GET (07-Jan-2023 04:28:10.675) (total time: 647ms):
Trace[1434669929]: ---"Writing http response done" 638ms (04:28:11.322)
Trace[1434669929]: [647.511421ms] [647.511421ms] END
E0107 04:28:36.196783       1 available_controller.go:527] v3.projectcalico.org failed with: failing or missing response from https://10.104.159.195:443/apis/projectcalico.org/v3: Get "https://10.104.159.195:443/apis/projectcalico.org/v3": net/http: request canceled while waiting for connection (Client.Timeout exceed
ed while awaiting headers)
W0107 04:28:37.224548       1 handler_proxy.go:106] no RequestInfo found in the context
E0107 04:28:37.224682       1 controller.go:113] loading OpenAPI spec for "v3.projectcalico.org" failed with: Error, could not get list of group versions for APIService
I0107 04:28:37.224734       1 controller.go:126] OpenAPI AggregationController: action for item v3.projectcalico.org: Rate Limited Requeue.
W0107 04:28:37.224763       1 handler_proxy.go:106] no RequestInfo found in the context
E0107 04:28:37.224913       1 controller.go:116] loading OpenAPI spec for "v3.projectcalico.org" failed with: failed to retrieve openAPI spec, http error: ResponseCode: 503, Body: service unavailable
, Header: map[Content-Type:[text/plain; charset=utf-8] X-Content-Type-Options:[nosniff]]
I0107 04:28:37.226228       1 controller.go:129] OpenAPI AggregationController: action for item v3.projectcalico.org: Rate Limited Requeue.
```

```sh kl kube-controller-manager-pi0
W0107 04:29:20.190372       1 garbagecollector.go:752] failed to discover some groups: map[projectcalico.org/v3:the server is currently unable to handle the request]
E0107 04:29:33.808084       1 resource_quota_controller.go:417] unable to retrieve the complete list of server APIs: projectcalico.org/v3: the server is currently unable to handle the request
W0107 04:29:50.245042       1 garbagecollector.go:752] failed to discover some groups: map[projectcalico.org/v3:the server is currently unable to handle the request]
E0107 04:30:03.850609       1 resource_quota_controller.go:417] unable to retrieve the complete list of server APIs: projectcalico.org/v3: the server is currently unable to handle the request
E0107 04:30:04.389367       1 namespace_controller.go:157] deletion of namespace client failed: unable to retrieve the complete list of server APIs: projectcalico.org/v3: the server is currently unable to handle the request
E0107 04:30:04.393223       1 namespace_controller.go:157] deletion of namespace management-ui failed: unable to retrieve the complete list of server APIs: projectcalico.org/v3: the server is currently unable to handle the request
E0107 04:30:04.397550       1 namespace_controller.go:157] deletion of namespace stars failed: unable to retrieve the complete list of server APIs: projectcalico.org/v3: the server is currently unable to handle the request
```
---
### 다른 노드에 있는 파드간 통신이 안되는 경우

`ipipMode` 혹은 `vxlanMode` 가 `Always` 로 사용되고 있다면 `CrossSubnet` 으로 변경해준다
`Always` 는 동일 서브넷에서도 패킷을 오버레이 네트워크 패킷으로 인캡슐레이션한다

+ https://projectcalico.docs.tigera.io/reference/resources/ippool

```sh
kubectl edit ippools.crd.projectcalico.org default-ipv4-ippool`
```
```yaml
ipipMode: CrossSubnet
```

## link
- [[kubernetes]]
- [[calicoctl]]
