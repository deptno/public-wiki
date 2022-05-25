# tmux

- https://tmuxcheatsheet.com

## copy
`ctrl + b`, [ -> ctrl + space -> y

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

## 세션
```tmux
:new -s session_name
```
```sh
tmux attach
tmux a
tmux a -t 0
tmux kill-session -t 0
```

## scroll back 저장
- https://newbedev.com/write-all-tmux-scrollback-to-a-file

```tmux
:capture-pane -S -65536
:save-buffer filename.txt
:delete-buffer
```

## plugin
- tpm - tmux plugin manager
- tmux - theme, dracula/tmux, fork -> deptno/tmux
- tmux-jump - easy move
- tmux-open - open
- tmux-thumbs - 환면에 있는 [[text]] 청크를 복사
- tmux-resurrect - 세션 창 분할 저장
