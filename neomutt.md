# neomutt

guide: https://neomutt.org/guide/

## 설정
- [[path|~/.config/neomutt/neomuttrc]]
```
# neomuttrc

source external_config.muttrc

set from = "deptno@gmail.com"
set realname = "Bonggyun Lee"

set mailcap_path        = "~/.config/neomutt/mailcap"
set folder              = imaps://imap.gmail.com/
set spoolfile           = "+INBOX"
set record              = "+[Gmail]/Sent"
set postponed           = "+[Gmail]/Drafts"
set mbox                = "+[Gmail]/All Mail"
set mbox                = "+[Gmail]/Trash"

# https://security.google.com/settings/security/apppasswords
set smtp_url = "smtp://deptno@gmail.com@smtp.gmail.com:587/"
set smtp_pass = "[PASSWORD]"
set imap_user = "deptno@gmail.com"
set imap_pass = "[PASSWORD]"

set sidebar_visible
set sidebar_format = "%B%?F? [%F]?%* %?N?%N/?%S"
set mail_check_stats

set sort=threads
set sort_browser=date
set sort_aux=reverse-last-date-received

mailboxes +INBOX +[Gmail]/Sent +[Gmail]/Drafts "+[Gmail]/All Mail" +[Gmail]/Trash 
```
- [[path|~/.config/neomutt/mailcap]]

## 어플리캐이션용 비밀번호 생성
1. gmail.com
2. 우상단 프로필 사진 클릭
3. Manage Your Google Account
4. Security
5. Signing in to Google
6. App passwords

## 사용법
- j/k - 위아래 메일
- v - 첨부파일 보기

## related
- [[mutt]]
- [[finicky]]
- [[email]]
