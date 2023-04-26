# harbor

container registry
+ https://goharbor.io

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


## related
- [[kubernetes]]
- [[book/24-steps-k8s]]
- [[quay]]
- [[docker]]
- [[podman]]
- [[traefik]]