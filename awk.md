# awk

+ https://www.theunixschool.com/2012/07/awk-10-examples-to-read-files-with.html

```sh
stream | awk '{print $2}'
stream | awk '{print $2 $1}'
stream | awk '{print $2 "\t" $1}'
awk -F'|' '{ print $2 $1 }' # 델리미터를 `|` 로 설정
awk -F'[+-]' '{ print $2 $1 }' # delimiter 가 2개
```

## link
- [[terminal]]
- [sed](sed)
