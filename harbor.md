# harbor

container registry
+ https://goharbor.io

## private 레포지터리 사용
```sh
docker build -t harbor.example.com/test/hello:latest
docker login harbor.example.com
docker push harbor.example.com/test/hello:latest
```

### kubernetes 에서 private 이미지 pull 하기 :private:
1. harbor web 에 접속해서 robot 계정을 생성, project 안에서 설정해도 상관없고, 관련된 권한을 적절히 부여
2. 생성된 token 을 password 로 사용하여 docker-registry secret 의 `password` 로 사용하여 계정 생성
3. `pod.spec.imagePullSecrets` 에 명시

```sh 
kubectl create secret docker-registry harbor -bot \
  --docker-server=https://harbor.example.com
  --docker-username=[bot name]
  --docker-password=[bot token]
```
```yaml
containers:
- name: hello
  image: harbor.example.com/test/hello:latest
imagePullSecrets:
- name: harbor-bot
```

## error
### image push, pull error
```sh
Error: trying to reuse blob sha256:8e012198eea15b2554b07014081c85fec4967a1b9cc4b65bd9a4bce3ae1c0c88 at destination: failed to read from destination repository test/image_name: 500 (Internal Server Error)
```
```sh 
The push refers to repository [harbor.deptno.dev/test/image_name]
ef3fca5020c3: Retrying in 1 second
c1f5993c08fb: Retrying in 1 second
0c8fc885a0f0: Retrying in 1 second
c2debf87e43a: Retrying in 1 second
c890fdde5b6c: Retrying in 1 second
a44f831aabcc: Waiting
5f70bf18a086: Waiting
58c6b0bd90b7: Waiting
53ca831d1016: Waiting
b89b9c6e1861: Waiting
b7e3600bfeb3: Waiting
c7e43350508a: Waiting
9b279096649b: Waiting
c2a86085bb2a: Waiting
6649379ee3b2: Waiting
2277bc8d4e09: Waiting
ed6682c37f64: Waiting
395626b7a3b8: Waiting
8e012198eea1: Waiting
received unexpected HTTP status: 200 OK

$ podman pull harbor.deptno.dev/test/hello-world:test                                                                                                                                                                                                                                                         INT  16.15.0 node  10:26:30
Trying to pull harbor.deptno.dev/test/hello-world:test...
Error: initializing image from source docker://harbor.deptno.dev/test/hello-world:test: invalid character '<' looking for beginning of value
```

[[traefik]] 을 통해 ingress routing 을 하고 있었는데, 중간에 middleware 를 통해 forward-auth 인증을 껴넣으면서 문제가 발생했다.
- image: thomseddon/traefik-forward-auth:latest
- 자체 인증이 있기 때문에 forward-auth 를 제거했다.

### traefik tls
tarefik 에서 tls 발급에 실패하는 경우 harbor 에 접근하지 못해서 이미지 pull 이 실패하면서 모든 파드가 못뜨는 문제가 있다.

```sh 
$ sudo vi /etc/containerd/config.toml
```
```toml
[plugins."io.containerd.grpc.v1.cri".registry.mirrors]
  [plugins."io.containerd.grpc.v1.cri".registry.mirrors."harbor.example.com"]
    endpoint = ["http://harbor.example.com"]
[plugins."io.containerd.grpc.v1.cri".registry.configs."harbor.example.com".tls]
  insecure_skip_verify = true
```
```sh 
sudo systemctl restart containerd
```

### ImagePullBackOff
> local 에서 kubernetes 에 private repository 에 있는 image 를 통해 띄울 때 발생
+ [[diary:2023-12-10]]
- private 레포지터리를 사용할때 이슈가 발생
- yaml 을 통해서 `imagePullSecrets` 를 지정하는 경우문제가 되지 않는다
- [[shell]] command 를 통해서 진행할때 [[kubernetes]] 는 이를 모르기 때문에 `imagePullSecrets` 옵션을 지정해주어야한다
```sh 
kubectl run test --image=harbor.registry.com/cns/name:tag --overrides='{"spec":{"imagePullSecrets":[{"name": "harbor"}]}}'
```

### Failed to dial harbor-notary-signer:7899: connection error: desc = "transport: x509: certificate has expired or is not yet valid: current time
```sh 
Failed to dial harbor-notary-signer:7899: connection error: desc = "transport: x509: certificate has expired or is not yet valid: current time
```
- [[helm]] chart 1.11.0 에서 1.14.0 으로 업그레이드하면서 해결하려고해보니 notary 가 안보임
- 업그레이드 중에 에러가 나서 `helm delete harbor` 이후 새로운 버전으로 생성(values.yaml 은 복사)
- 기존 pvc 들이 모두 제대로 붙으면서 해결됨

## link
- [[kubernetes]]
- [[book/24-steps-k8s]]
- [[quay]]
- [[docker]]
- [[podman]]
- [[traefik]]
