# ssh

## 맥에서 ssh 접속 허용하기
시스템 환경설정 -> 공유 -> 원격 로그인(켬)

## ssh public key 를 이용한 접속
> ssh key 가 있다고 가정
```sh
$ ssh-copy-id -i ~/.ssh/[KEY_NAME_eg_id_rsa].pub [USER]@[HOST]
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
Password:

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh '[USER]@[HOST]'"
and check to make sure that only the key(s) you wanted were added.
```

## ssh host name alias
```text `~/.ssh/config`
host [HOST_NAME_ALIAS]
  hostname [HOST_ADDRESS]
  user [USER]
```

### ssh public key + ssh host name alias 를 조합시 비밀번호 입력없이 접근 가능
```sh
$ ssh [HOST_NAME_ALIAS]
```
