# ssh

패스워드 없이 키를 통해서 접근하고자 한다면 [[ssh-copy-id]] 페이지를 참고한다.

## 맥에서 ssh 접속 허용하기
시스템 환경설정 -> 공유 -> 원격 로그인(켬)

## ssh host name alias
```text
host [HOST_NAME_ALIAS]
  hostname [HOST_ADDRESS]
  user [USER]
```
`~/.ssh/config`

### ssh public key + ssh host name alias 를 조합시 비밀번호 입력없이 접근 가능
```sh
$ ssh [HOST_NAME_ALIAS]
```

## link
- [[scp]]
- [[ssh-copy-id]]
- [[ssh-keygen]]
