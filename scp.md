# scp

remote <-> local 간의 파일 전송

```ssh
scp -r [USER]@[HOST]:/path/to/remote/dir /path/to/local/dir
scp /path/to/local/file [USER]@[HOST]:/path/to/remote/dir
```

## options
- `-r` recursive
- `-p` preserve 수정 시간을 소스로 유지시킨다
- `-i` identity file, password 대신 key file 지정
- `-P` port 지정, 22 가 아닌경우 사용한다.

`HOST` 는 `~/.ssh/config` 를 준수한다. 때문에 `ssh-copy-id` 등을 통해 키를 설정해 두면 비번없이 편하게 복사가 가능.

## related
- [[ssh]]
- [[port]]
