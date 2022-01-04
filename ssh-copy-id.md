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

## releated
- [[ssh]]
- [[scp]]
