# curl

```sh
curl -X PUT -d '{"on": true}' -H "Content-Type: text/plain;charset=UTF-8" http://192.168.0.222/api/xxx/lights/2/state\

curl 'http://192.168.0.222/api/xxx/lights/2/state' \\
  -X 'PUT' \\
  -H 'Connection: keep-alive' \\
  -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36' \\
  -H 'Content-Type: text/plain;charset=UTF-8' \\
  -H 'Accept: */*' \\
  -H 'Origin: http://192.168.0.222' \\
  -H 'Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7' \\
  --data-raw '{"on":false}' \\
  --compressed \\
  --insecure
```

## options
- I, --head - fetch response headers only
- i, --head - fetch headers + 응답 값
- s, --silent
- o, --output <file> - stdout 이 아닌 파일로 응답을 저장
- w, --write-out - display information after a completed transfer
- X, - PUT 등 메소드 지정
- d @filename,  - json 등을 body 보낼때 사용

## link
- [[terminal]]
- [[http]]
- [[request]]
- [[fx]]
