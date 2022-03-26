# jql

jira query language

## recipe
- 스크럼 팀원용
```jql
project = "B2C" and (assignee in ("deptno") OR participants in ("deptno")) ORDER BY created DESC
```
- 기간 검색
```jql
(assignee = "deptno" OR Participants in (deptno)) AND project = "PROJECT_NAME" AND created > startOfMonth("-6M") ORDER BY created DESC
```

## related
- [[jira]]
