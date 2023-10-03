# vim-startify

## 설정
- neovim 을 쓰면서 해당 플러그인을 [[lua]] 대체하지 않고 사용하기로 함
- 이전에는 북마크로 설정파일들을 사용하는 정도의 용도를 사용
- 이번에는 북마크 + session 설정을 잘 쓸 수 있도록 조절함

### tmux window -> startify session
- as-is 
  - tmux session
    - 개인
    - 회사
  - tmux window
    - git repository 별로 session 당 10개에 달할 때도 있음
    - session 마다 **vim window 가 존재**
  - tmux pane
    - 필요한 만큼의 pane 로 구성, 레포당 여러 서버를 띄우곤 해야함
- to-be
  - tmux session + window 의 구성을 -> startify session 으로 관리
  - git log 나 server log 등의 관리는 때마다 유동적으로 tmux window 를 생성해서 처리
  - project, [[vimwiki]], config 설정 등을 startify session 으로 저장
  - active startify session 과 관계없이 vim 을 exit 하는 경우에는 `_latest` session 에 저장
  - session, bookmark 는 숫자가 아닌 고정 문자 indicies 를 설정해서 빠르게 열수 있도록 설정
    - `_latest` startify session 을 포함해서 session 과 bookmark 포함
- as-is 문제
  - tmux session 이나 window 를 이동하는 것은 기존에도 문제가 되지 않았음
  - vim 을 세션 마다 열어두다보니 코드를 보는 윈도우에서 vim 을 잠깐 띄우거나, 그 상태에서 설정 파일을 수정하고자 bookmark 된 파일을 여는 경우 swp 파일 문제등이 발생
  - vim 자체가 여러개 열리는 것도 불필요
  - session + window 가 너무 많다보니 비활성화된 고정 window 도 비효율
- to-be 장점
  - vim 을 하나로 가져가니 vim 내에서 session 으로 빠르게 이동
  - swp 문제 제거
  - 수정하던 곳을 찾기 위해 tmux session + window 를 찾아가야하는 번거로움이 덜어짐
  - tmux 가 클리너 되어 작은 모니터 가독성 향상
  - vim single instance
- 작업하는 김에 조금 공부도 해본 [[lua]] 로 기본 설정 함수들을 포팅해봄
- 아쉬운 점
  - vim-startify session 이 tabpage 를 함께 처리하고 있기 때문에 두 session 을 서로 참조하기 어렵다
    - `_latest` session 고도화를 해려했으나 autocmd 가 모잘라서 포기했다.
    + https://github.com/mhinz/vim-startify/issues/468

## 기본 숏컷
- `e` - 빈 버퍼로 normal mode
- `i` - 빈 버퍼로 insert mode

## 시작화면에서 마킹
- `s` - split horizontal
- `v` - split vertical
- `b` - buffer
- `t` - tab

후 엔터치면 해당 컨셉으로 열릴 것만 같음

## link
- [[vim]]
- [[neovim-setup]]
