# tr

문자열 변환/삭제

## 변환
```sh
$ echo '"/hello/UPPERCASE"' | tr 'a-zA-Z' 'A-Za-z'
"/HELLO/uppercase"
```

'\n' 혹은 ' ' 으로 변경하면 [[shell-script]] 에서는 배열을 만들어 사용이 가능
```sh
$ echo 'a/b/c/d' | tr '/' '\n'
a
b
c
d
```

## 삭제
```sh
$ echo '"/hello/UPPERCASE"' | tr -d '"/a-z"'

UPPERCASE
```

## related
- [[terminal]]
- [[sed]]
- [[shell-script]]
