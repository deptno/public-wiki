# sed

- cheatsheet: https://quickref.me/sed

deletemiter 
- /
- :
- |

구분자를 바꿔서 가독성을 향상 시킬 수 있다
```sh
$ echo "/hello/world" | sed 's/\/hello\//'
world
$ echo "/hello/world" | sed 's:/hello/::'
world
$ echo "/hello/world" | sed 's|/hello/||'
world
```

## usage
```sh 
sed '$d' # 여러줄의 결과중 마지막 라인을 제거한다
```

## link
- [[terminal]]
- [[tr]]
- [[grep]]
- [[awk]]
