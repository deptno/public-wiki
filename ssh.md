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

### github 에서 여러계정 사용시
```sh 
git clone git@[HOST]:repopath
```
- `~/.ssh/config` 에 저장된 호스트 네임과 일켜야한다

## 포트포워딩
```sh 
ssh -NL localhost:8888:localhost:8000 user@server
# -f 옵션이 추가되면 background 실행
ssh -NLf localhost:8888:localhost:8000 ssh_session_name
```

## link
- [[scp]]
- [[ssh-copy-id]]
- [[ssh-keygen]]
- [[wsl]]
- [[github]]
- [[sftp]]
- [[dvc]]
