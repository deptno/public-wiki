# tmux

- https://tmuxcheatsheet.com

## copy
`ctrl + b` [ -> ctrl + space -> y

## 중첩 tmux
tmux 에서 ssh 로 들어가 다시 tmux 를 사용하는 경우에는 prefix2 + key 를 통해 사용할 수 있다.

기본키인 prefix(ctrl + b) 인 경우
| key                 | target |
|---------------------|--------|
| prefix + w: local   | local  |
| prefix + prefix + w | remote |

## 기본 디렉토리 변경
```tmux
:attach -c ~/path
```

## 설정 리로딩
```tmux
:source-file ~/.tmux.conf 
```

## 세션 생성
```tmux
:new -s session_name
```

## scroll back 저장
- https://newbedev.com/write-all-tmux-scrollback-to-a-file

```tmux
:capture-pane -S -65536
:save-buffer filename.txt
:delete-buffer
```
