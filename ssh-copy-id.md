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

## 정리전

```sh 
ssh 192.168.0.7
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:830HsfVL0dYgAqKy0TL+mu5J9JBobI88H2tXl9rWDf0.
Please contact your system administrator.
Add correct host key in /home/pi/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /home/pi/.ssh/known_hosts:3
  remove with:
  ssh-keygen -f "/home/pi/.ssh/known_hosts" -R "192.168.0.x"
Host key for 192.168.0.x has changed and you have requested strict checking.
Host key verification failed.

ssh-keygen -f "/home/pi/.ssh/known_hosts" -R "192.168.0.x"

pi@pi0:/etc/kubernetes/pki$    ssh-keygen -f "/home/pi/.ssh/known_hosts" -R "192.168.0.x"
# Host 192.168.0.x found: line 1
# Host 192.168.0.x found: line 2
# Host 192.168.0.x found: line 3
/home/pi/.ssh/known_hosts updated.
Original contents retained as /home/pi/.ssh/known_hosts.old

```

## releated
- [[ssh]]
- [[scp]]
