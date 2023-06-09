# scp

remote <-> local 간의 파일 전송

```sh
scp -r [USER]@[HOST]:/path/to/remote/dir /path/to/local/dir
scp /path/to/local/file [USER]@[HOST]:/path/to/remote/dir
```

## options
- `-r` recursive, directory 복사시 사용
- `-p` preserve 수정 시간을 소스로 유지시킨다
- `-i` identity file, password 대신 key file 지정
- `-P` port 지정, 22 가 아닌경우 사용한다.
- [ ] `-O`
  
`HOST` 는 `~/.ssh/config` 를 준수한다. 때문에 `ssh-copy-id` 등을 통해 키를 설정해 두면 비번없이 편하게 복사가 가능.

## error
```sh 
subsystem request failed on channel 0
scp: Connection closed
```
scp 명령어가 실패했는데 이 때는 -O 옵션을 추가해서 성공했다.
  + https://stackoverflow.com/questions/74311661/subsystem-request-failed-on-channel-0-scp-connection-closed-macbook
---

## link
- [[ssh]]
- [[port]]
