# gpg

## 생성
> password 등록시에는 `GPG_TTY=$(tty)` 가 필요, 외에도 tmux 와 다소 충돌이 있어서 사용하지 않기로 함
```sh
gpg --full-generate-key # RSA(0)
gpg --list-secret-keys
gpg --armor --export SECRET_KEY_ID_XXXXXXXXXXXXXXXXXXXXXXXXXX | pbcopy # github 등록
```

## 갱신
gpg key 키가 만료되어 에러가 난다
```sh 
gcam "feat: add helm chart traefik 20.8.0" --verbose
error: gpg failed to sign the data
fatal: failed to write commit object
```
키를 갱신한다. github 에서는 이미 등록되어있는 키 그대로 사용이 가능하다.
```sh
$ gpg --list-secret-keys

/Users/deptno/.gnupg/pubring.kbx
--------------------------------
sec   rsa4096 2022-01-04 [SC] [expired: 2023-01-04]
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
uid           [ expired] 이봉 <deptno@gmail.com>

sec   ed25519 2022-03-01 [SC]
      BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB
uid           [ultimate] 이봉 (mbp14) <deptno@xxxxxxx.com>
ssb   cv25519 2022-03-01 [E]

$ gpg    ok  16.15.0 node  11:00:37
gpg: WARNING: no command supplied.  Trying to guess what you mean ...
gpg: Go ahead and type your message ...
^C
gpg: signal Interrupt caught ... exiting

$ gpg --edit-key gpg
$ gpg --list-keys
/Users/deptno/.gnupg/pubring.kbx
--------------------------------
pub   rsa4096 2022-01-04 [SC] [expired: 2023-01-04]
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
uid           [ expired] 이봉 <deptno@gmail.com>

pub   ed25519 2022-03-01 [SC]
      BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB
uid           [ultimate] 이봉 (mbp14) <deptno@zigbang.com>
sub   cv25519 2022-03-01 [E]

$ gpg --edit-key AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
gpg (GnuPG) 2.3.8; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  rsa4096/A20921D2906CB572
     created: 2022-01-04  expired: 2023-01-04  usage: SC
     trust: ultimate      validity: expired
ssb  rsa4096/2F36802B8CE1A693
     created: 2022-01-04  expired: 2023-01-04  usage: E
[ expired] (1). 이봉 <deptno@gmail.com>

gpg> expire
Changing expiration time for the primary key.
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at 일  1/14 11:03:12 2024 KST
Is this correct? (y/N) y

sec  rsa4096/A20921D2906CB572
     created: 2022-01-04  expires: 2024-01-14  usage: SC
     trust: ultimate      validity: ultimate
ssb  rsa4096/2F36802B8CE1A693
     created: 2022-01-04  expired: 2023-01-04  usage: E
[ultimate] (1). 이봉 <deptno@gmail.com>

gpg: WARNING: Your encryption subkey expires soon.
gpg: You may want to change its expiration date too.
gpg> key 1

sec  rsa4096/A20921D2906CB572
     created: 2022-01-04  expires: 2024-01-14  usage: SC
     trust: ultimate      validity: ultimate
ssb* rsa4096/2F36802B8CE1A693
     created: 2022-01-04  expired: 2023-01-04  usage: E
[ultimate] (1). 이봉 <deptno@gmail.com>

gpg> expire
Changing expiration time for a subkey.
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at 일  1/14 11:05:48 2024 KST
Is this correct? (y/N) y

sec  rsa4096/A20921D2906CB572
     created: 2022-01-04  expires: 2024-01-14  usage: SC
     trust: ultimate      validity: ultimate
ssb* rsa4096/2F36802B8CE1A693
     created: 2022-01-04  expires: 2024-01-14  usage: E
[ultimate] (1). 이봉 <deptno@gmail.com>

gpg> save

$ gcam "feat: add helm chart traefik 20.8.0"
[master (root-commit) dee4a99] feat: add helm chart traefik 20.8.0
 46 files changed, 10882 insertions(+)
 create mode 100644 traefik/.helmignore
```

key 1 도 선택해서 subkey 도 함께 갱신해주도록한다. 이후에는 커밋이 된다.



## link
- [[setup-terminal]]
