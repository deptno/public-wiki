# yq

## 설치
```sh
brew install yq
```

## 사용
```sh
cat some.yaml | yq '.scripts'
cat some.yaml | yq '... comments=""'.scripts' # 주석 제거 후 프린트
```

## related
- [[jq]]
