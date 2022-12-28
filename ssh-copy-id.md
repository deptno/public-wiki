# ssh-copy-id
  
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

## error
```
Permission denied (publickey).
```
- .ssh, .ssh/* permission 확인
- /etc/hosts 에서 아는 호스트 인지 확인, ssh/config 에 있어도 됨
- .ssh/config User 가 제대로 설정되어있는지 확인
- .ssh/authorized_keys 에 제대로 pub 키가 들어가있는지 확인

## releated
- [[ssh]]
- [[scp]]
