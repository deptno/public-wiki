# jq

## 설치
```sh
arch -arm64 brew install jq
```

## 사용
```sh
cat package.json | jq '.scripts'
cat package.json | jq '.scripts.start'
```
