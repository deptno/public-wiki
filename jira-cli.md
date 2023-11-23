# jira-cli
> go 로 만들어진 jira cli

- 터미널에서 markdown 으로 티켓 내용 확인가능
- 티켓생성이나 모니터링 가능

## usage
- QA 시작시 QA 티켓을 가지고 터미널 2분할하여 사용가능
- 기타 담당자 설정가능
```sh 
jira issue list -P QA-TICKET-ID # QA밑 티켓
jira issue list -P QA-TICKET-ID -s~Done -s~Backlog # 완료되었거나 Backlog 로전환되지 않은 티켓만 모니터링
```

## link
- [[go]]
- [[jira]]
- [[terminal]]
