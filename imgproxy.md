# imgproxy

+ https://docs.imgproxy.net/installation

```sh
helm repo add imgproxy https://helm.imgproxy.net/
helm pull 
helm pull imgproxy/imgproxy --untar
helm upgrade imgproxy imgproxy --install --create-namespace -n imgproxy
```

## related
- [[helm]]
- [[kubernetes]]
- [[nginx]]
