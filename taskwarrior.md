# taskwarrior

## command
- `add` : 추가
- `modify` : 수정
- `done` : 완료
- `delete` : 삭제, 완료 여부와 관계가 없고 여전히 저장상태다.
- `start` : 시작
- `stop` : 중지
- `annotate` : 코멘트를 하나 추가한다.
- `undo` : 최근작업 하나를 되돌린다.
- `purge` : delete 된 task를 완전히 삭제한다.
- `info` : 타스크 정보를 출력한다.

### print
- `history` : 테이블로 현황을 보여준다.
- `ghistory` : 그래프로 현황을 보여준다.
- `calendar` : 일정이 등록된 날들을 하이라이트하는 캘린더를 노출한다.
- `timesheet` : 주단위로 리스팅
- `burndown` : 번다운 차트를 보여준다. 여러 기간 옵션이 있다.

#### list
- `overdue` : duedate 이 지난 타스크
- `newest` : 최신 등록순으로 나열
- `due:today` : 오늘이 due day 인 것들 나열

```sh
task TASK_ID [COMMAND] [ARGS]
```

### add
```sh
task add +tag "TODO"
task add +VIRTUAL_TAG "TODO"
task add schedue:today "TODO"
task add due:tommorow "TODO"
task add until:+3days "TODO"
task add priority:H "TODO"
task add project:home "TODO"
```

### modify
```sh
task TASK_ID modify +TAG
task TASK_ID modify +TAG schedue:today due:tommorow until:+3days "DESCRIPTION"
```

### list
```sh
task next # default
task waiting
task long
task all +DELETED
task status:pending -WAITING limit:page
task +tag +tag -tag
```

### context
> 자주쳐야하는 tag 혹은 project 등을 잠시 context에 넣어서 편하게 사용한다. read, write 모두에 사용될 수 있다.

```
task context define CONTEXT_NAME +tag +VIRTUAL_TAG + project:PROJECT_NAME due:today project:deptno
task context CONTEXT_NAME
task context none # 해제
```

### 시간 개념
- `schedule` - 일정의 시장
- `due` - 해당일 
- `until` - 마감일
[[-]] `wait` - 이날 전까지는 노출하지 않는다.

책 반납을 `schedule` 부터 가능하고 계획한 예정일은 `due` 이며 연체료를 물지 않으려면 `until` 까지는 해야한다.  
그리고 `wait` 시점까지는 목록에서 노출되기를 원하지 않는다.
