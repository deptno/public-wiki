# jq

## 설치
```sh
arch -arm64 brew install jq
```

## 사용
```sh
cat package.json | jq '.scripts'
cat package.json | jq '.scripts.start'
jq -n '{"json": "data"}' # formating
```

## format
### array
- https://stedolan.github.io/jq/manual/
```sh
... | jq '.[] | .name, .version'              # 멀티 라인
... | jq '.[] | [.name, .version]'            # 어레이 텍스트 그대로 출력
... | jq '.[] | [.name, .version] | @tsv'     # \t
... | jq '.[] | [.name, .version] | @csv'     # ,
... | jq '.[] | [.name, .version] | @html'
... | jq '.[] | [.name, .version] | @text'
```

## link
- [[vim]]
- [[gh]]
- [[xsv]]
- [[yq]]
