# nginx

```sh
Error from server (BadRequest): error when creating "config/nginx-static.yaml": ConfigMap in version "v1" cannot be handled as a ConfigMap: json: cannot unmarshal object into Go struct field ConfigMap.data of type string
Error from server (BadRequest): error when creating "config/nginx-static.yaml": ConfigMap in version "v1" cannot be handled as a ConfigMap: json: cannot unmarshal string into Go struct field ConfigMap.data of type map[string]string
```

아래와 같은 형태에서 에러가 발생했다
```yaml
kind: Config
apiVersion: v1
metadata:
  name: nameof
data:
  nfs:
    server: value
    path: value
```
아래와 같은 형식으로 수정한다
```yaml
# ...
data:
  nfs: |
    server: value
    path: value
```
[[helm]] chart 에 정의된 타입에 종속적일텐데 차트에는 아래와 같이 선언되어있다
```tpl
configMap:
  name: {{ printf "%s" (tpl .Values.staticSiteConfigmap $) -}}
```
## link
- [[kubernetes]]
- [[helm]]
