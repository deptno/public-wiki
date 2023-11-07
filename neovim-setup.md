# neovim-setup
 
## nvchad
- 시작 설정으로 nvchad 선택
- custom config
  + https://github.com/deptno/neovim-config-for-nvchad
    - lspconfig 포함

## tree-sitter 모든 언어 설치
```sh
:TSInstall all
```

## issue
- which-key 에 [[vimscript]] 를 통해서 등록한 매핑은 보여지지 않는 것으로 보임.

## 개선사항 [[@todo]]
> [[@todo]] 에서 이관

- [X] *bug* gx url 에서 `?` 가 존재하면 열리지 않음 + https://github.com/deptno/nvim/commit/f4e09cfbf9abf1ad52bb952c63727d34c4941cfc
- [ ] [bun](bun) 을 사용해서 [typescript](typescript) 테스트 환경 구축
- [X] session 이동시 열린 버퍼를 가지고 이동하는 문제+ https://github.com/deptno/nvim/commit/04d8e15209b2052892a164db47a174fa2a06e51a
  - [X] 후행으로 닫도록 수정, 속도관련해서도 이슈가 있음
- [ ] jira ticket 발행
- [X] harpoon + https://github.com/deptno/nvim/commit/9ab9f786253b76069f1ad7d53ba9a3bb39aa5808
- [X] *bug* ;db 를 통한 quickfix 류의 창이 열린 이후로 q 누르면 버퍼 닫힘
  + 이벤트 호출을 막는 법은 찾지 못해서 이벤트를 한번만 처리하도록 처리 + https://github.com/deptno/nvim/commit/66836ec2098f5268b2d8c838f99400070293f995
- [ ] leetcode.nvim
- [o] lsp
  - [X] 포매팅 guard.nvim
  - [X] 파일명 변경, nvim-lsp-file-operations -> 동작 안함
  - [ ] lsp 를 통한 영어단어 hover + gpt
- [X] git ~~furgitive~~ blame 때문, blame + git-blame 추가 + https://github.com/deptno/nvim/commit/62c6da867e8e2b5846afbe6c3461da633396102a
- [ ] tmux-popup command 생성 input: shell command i.g.) tig blame
- [ ] 마크다운 블럭 내에서의 언어 실행 + [nvim treesitter](nvim-treesitter) 
  - [ ] 블럭에 대해서 repl 플러그인 검색
- [X] [vimwiki](vimwiki) syntax highlight 는 markdown 을 filetype 은 vimwiki 를 유지 + https://github.com/deptno/nvim/commit/2c6d5605ffc2339f10164877573e1fe05053d797
+ https://github.com/ecosse3/nvim
  - [X] nvim-dap + https://github.com/deptno/nvim/commit/abff6147f65b87c9ee4deaa5782b04d15d1969e2
  - [X] react-native 디버깅 가능한지 파악 + https://github.com/deptno/nvim/commit/01478edf8a3863cdb933e7a1e518c582e20f619f
    - mason 을 통해서도 인스톨이 가능할 수 있을것 같음
  - [ ] [jest](jest)
  - [ ] [rust](rust)
- [X] *wip* react-native 같은 특수 파일 suffix 지원 `.native.tsx` -> `.tsx` 인식 + https://github.com/deptno/nvim/commit/dda74cfcada336b0eb80cb84e84baf8e441dacf5
  + https://github.com/neovim/neovim/issues/20784#issuecomment-1722288456
- [X] lspsaga
  - [X] keymap
    - [X] `[e` diagnostic error
      - [X] *fail* `[d` diagnostic -> 메소드 없음 
    - [X] `K` hover
    - [X] `<leader>rA` rename ++project 
    - [X] `<leader>2` outline > symbols-outline
    - [X] `gd` definition
    - [X] `gD` peek
      - [X] *drop* `gtd` type
      - [X] *drop* `gtD` peek
      - [X] *drop* `gi` peek
    - [X] `<leader>ic` incoming_calls
      - [X] *drop* `<leader>oc` outgoing_calls, 이건 중요하지 않음
- [X] vim-rest-console 연동 + https://github.com/deptno/nvim/commit/059745591ce6f5e521ebbd07e676fd1c64de8680
- [X] [lazygit](lazygit) 연동 + https://github.com/deptno/nvim/commit/1dd2dc48067d9bf01f4484ae15fcfc1d62df7a5c
  - [X] https://github.com/gioele/vim-autoswap 설정과 함께 봐볼 것 + https://github.com/deptno/nvim/commit/9f3be8f546d191a459f3728ea7e06448681e050e
  - [X] 현재 파일의 디렉토리 기반으로 열기 + https://github.com/deptno/nvim/commit/32243e9516fc5e46f98212283a643bc91b52e387
- [X] fx 연동 + https://github.com/deptno/nvim/commit/3f125cae051fab1147dbc494cbfc3fae59773279
- [X] session
  - [X] auto-session -> persisted, mksession 은 간혹 세션의 이전 버퍼가 딸려오는 문제는 있는 것 같다 -> 세션 목록에서 저장하도록 해서 일단 사용
  - [X] *option* ~~session 은 버리고 directory 기반 이동 + telescope oldfiles 가 낫겠음, 프로젝트 이동간에 cwd 가 변경되는 경우가 너무 많음~~
  - [X] *option* persisted 를 기준으로 세션 이동 전에 세션을 자동 저장하도록 한다 + https://github.com/deptno/nvim/commit/a2bd474c5706bccd253bab88634acedfb844fa6a
- [X] quickfix vs loclist 차이 알아보기, quickfix:project, location:local 레벨, 파일단위인지 프로젝트 단위인지
- [o] plugin 추가
  - [o] nvim-ufo
    - [X] 설정 + https://github.com/deptno/nvim/commit/38f6c19
    - [ ] 폴드가 원하는대로는 안풀려서 좀더 찾아보자
  - [X] tabscope + https://github.com/deptno/nvim/commit/2341b5c
- [X] blankline 설정 + https://github.com/deptno/nvim/commit/94f9410d
- [X] gitsign
  - [X] ~~Change-base telescope 로 전환~~ -> [lazygit](lazygit) 으로 인해 니즈 죽음
  - [X] diff 에 관련한 설정이 가능한지 -> 가능하지 않음 delta 적용불가 + https://github.com/lewis6991/gitsigns.nvim/issues/723
- [X] telescope layout 변경 + https://github.com/deptno/nvim/commit/60ed157
- [X] telescope oldfiles 에서 cwd 기준으로만 리스팅 + https://github.com/deptno/nvim/commit/8536cd6dfdf0113de078eb4beccd0cc1711ce7c9
- [ ] nvchad pull
- [X] bookmark: 이슈별 그룹지정, 이슈리스트 J 로 테스트해보자
- [X] symbol, 찾기 단축키 + `<leader>ds`
- [X] bookmark 문자 컬럼 노출 - marks.nvim + https://github.com/deptno/nvim/commit/e2c36dd
- [X] navigate window ctrl+w + {h,j,k,l} -> 원래 됨 ctrl+{h,j,k,l}
- [X] resize window ctrl+{H,J,K,L} -> 생각보다 메커님이 복잡 윈도우 사용
- [X] emoji 관련 nvim-cmp 플러그인도 찾자 + https://github.com/deptno/nvim/commit/6b24994
- [X] [[html]] 작성하는데 [[zen-coding]] 안먹는다 플러그인 찾자, lsp 로 와 기본적으로 제공되는 snippet 으로 대충 가능할 것으로 보임
  - [X] emmet-language-server, tailwindcss-language-server 적용 + https://github.com/deptno/nvim/commit/fa5bbca972efe2c8b6e98e03e7b2cabb626046ed
- [X] luaJIT 참조하도록 설정 + https://github.com/deptno/nvim/commit/85619c9
- [X] vim 에서 cwd 혹은 현재 파일의 위치를 가지고 [[tmux]] {pane,window} 를 생성할 수 있도록 지원 - https://github.com/deptno/NvChad/commit/8e6dfa1
- [X] vim cd.. 이 ~~cwd 기준이라 recursive 하게 동작하지 않는 문제~~ 수정
  - [X] vim cd.. cwd 기준이 아니라 현재 파일 기준이라 동작하지 않는 것으로 cwd 기준으로 수정 필요
- [O] [[vim-startify]] 설정
  - [ ] startify-session 문제점
    - sesion 이동시에 tag stack 이나 이동 스택은 살아있음
      - ctrl+t, ctrl-o 등으로 해당 스택을 이동하는 경우 이전 session 의 파일들을 불러오게되면서 session 영역이 혼탁해짐
      - [[comparison:tagstack-vs-jump]]
      - [[vimwiki]] 에서 링크 이동에 tag stack 이 사용되지 않는 문제
        - [X] ctrl+i 가 `VimwikiNextLink` 로 동작하지 않도록 제거 -> `ctrl+i` 는 `tab` 과 같은 역할을 하며 이건 vim 잘못이 아님
  - [X] startify session 에서 cwd 지정이 가능한지 확인
    - [X] startify_change_to_dir  옵션을 사용해 봤으나 사용성이 떨어지고 git root 로 가야함 -> 범용적으로 cdp 구현 프로젝트로 루트 이동이 가능
      + https://github.com/deptno/nvim/commit/a869b4a4
  - [X] tmux window -> session
  - [X] head text 제거
  - [X] git modified 와 untracked 를 합칠 것
  - [X] 나갈때 임시 세션(`_latest`)으로 저장
  - [X] vim-startify 설정에 대한 기록 작성
  - [O] 옵션 검토
    - [X] g:startify_update_oldfiles - vim 시작시점이 아니라 계속적인 업데이트로 이해
    - [X] g:startify_session_persistence - 이렇게 되면 세션이 최종 상태를 기억하게됨, _latest 가 번잡하면 이걸 테스트
    - [X] 마지막 session 간의 전환 지원이 필요 https://github.com/deptno/nvim/commit/1996f7c
    - [X] ~~임시 세션 저장 슬롯 여러개 지원~~ - 의미 없을 것 같음
    - [X] g:startify_session_before_save - ~~nvim-tree~~, ~~symbol-outline~~ 등 제거, 둘다 문제가 있어서 제거 못함
      - https://github.com/deptno/nvim/commit/cf9c4a9
    - [X] cmd 지정이 가능함 - 마지막 세션간 전환 구현
    - [X] ~~session 에 속한 파일이 열리는 순간 해당 session 을 여는 기능 검토(이걸 쓰면 `_latest` 는 드랍)~~
    - [X] 나갈때 neogit 관련 파일이 열려있으면 닫고 이동하도록, NeogitStatus
      + https://github.com/deptno/nvim/commit/1499958
    - [X] ~~session 변경시 저장~~ 위험할 수 있을 것 같음, 저장을 습관화
    - [ ] repo 의 경우 branch 별 세션 저장 및 변경 방법 고민
  - [ ] session bookmark 가 필요한것
- [ ] cd?
  - [X] 현재 브랜치 + origin 까지 확인 필요 + https://github.com/deptno/nvim/commit/7fbcc6e
  - [ ] 현재 파일의 정보고 같이 보는게 좋겠음
- [X] @cite 자동완성 dictionary 생성 
  + https://github.com/deptno/nvim/commit/de370b0ad52e9121f30067069ecd27be13a6253c
  - .github/CODEOWNERS 로 시작 slack, github 
  - slack 은 api 보니 힘들거 같고 git 관련해서는 cmp_git 이 잘되어 있어서 요걸 쓰기로 함
- [X] [[nvchad]] default theme 변경
- [ ] 파일 네비게이션
  - [ ] 파일 이동간에 git_root 를 lcd 혹 cwd 설정
  - [ ] 혹은 git_root 가 다른 파일 이동에는 tabpage 전환, tabpage 마다 root
- [.] [[vimwiki]]
  - [ ] diary 간 이동할때 buffer 가 계속 늘어나지 않도록 조치
  - [X] diary 를 폴더구조거아니라 prefix 형태로 평탄화 가능한 옵션이 있는지 검토
    - [X] `diary:` prefix 를 통해 상대경로가 아니어도 링크가능하므로 이걸 이용
    - [X] 경로에 depth 가 들어간 경우, `/` prefix 를 통해서 wiki 루트로 부터 계산되니 이걸 사용
      - [X] [[deptno.dev]] 에서 해당 컨셉이 지원되고 있는지 확인이 필요 -> 안되면 구현
  - [ ] 회사와 private wiki 를 분리
    - [ ] 마이그레이션 - diary 를 통째로 복사하고([[git-subtree]]) 수동으로 필터([[git-rebase]])링해서 필터링할 것
  - [ ] *bug* 특정 케이스에 vimwiki가 꺼지는 상황이 있음 현재 파일 기준 24라인 이후에 뎁스가 더 들어간 todo를 생성하며 꺼짐
  - [X] timestamp, 혹은 다이어키를 삽입하는 키맵 추가 - 이미 snippet 자동완성으로 지원되고 있었음 `diso`
  - [ ] code block 을 실행할때 shell 구문인 경우 tmux 의 다른 panel에서 실행할 수 있도록 지원 [[gx]]
  - [ ] 다른 wiki 로 이동할 때 session 전환 [[vim-startify]]
  - [ ] calendar 를 이용해서 시각화
    - [ ] wiki
      - [ ] diary
      - [ ] 날짜 태그 혹은 다이어리 링크 태그
      - [ ] git stat since until 을 통해서 파일기록
    - [ ] 슬랙 해당날 태그등
    - [ ] apple reminder
    - [ ] api,구글 등 cloud provider 연동, filter + 휴가 이모지등
- [ ] [[octo]] 도입 검토
- [X] [[neogit]] 제거 https://github.com/deptno/nvim/commit/cc0fcebc0ef9cc282ad7dc1c49b4eb63148c77d6
  - [X] *remove* git checkout 대신 git switch 를 이용할 방법
  - [X] *remove* diffview 가 활성화되면 theme 자체를 변경해버리는 것도 고려
  - [X] *remove* change base 쉽게
- [.] window
  - [X] buffer 최대 가로사이즈로 window width 설정 https://github.com/deptno/nvim/commit/3008b87d
- [o] code chunk 실행, [[repl]]
  - [X] [[lua]] 에서 해당 라인을 실행할 수 있도록 처리 -> vim.notify https://github.com/deptno/nvim/commit/bd102e6
  - [ ] 언어별 핸들러 제공필요, ts bun
- [O] lab/gx [[gx]]
  - [X] github wiki 를 사용하고 있는데 이 부분 github link 따기 -> wiki repo 를 일반레포로 전환하고 wiki 는 drop, needs 없음
  - [X] ssh 기반으로 코드가 작성되어있는데 https 같이 지원 -> 테스트 해보니 이미 됨
  - [X] *bug* [n] repository 이름에 `.` 들어간경우 유지 필요 i.e. deptno.dev -> https://github.com/deptno/nvim/commit/6833181
  - [X] *feat* url 인데 일반 커밋으로 인식 + https://github.com/deptno/nvim/commit/c366ead
  - [X] *bug* cd 정보에 `]` 마지막 괄호가 빠짐 + https://github.com/deptno/nvim/commit/10c043d0
  - [X] *bug* url 에 `.` 들어간경우 제거됨 i.e. + https://github.com/deptno/nvim/commit/c27ad96
  - [X] [vV] current file 기준으로 gx 를 열어줘야하는데 cwd 기준으로 열어주고있음 + https://github.com/deptno/nvim/commit/93c4c13
  - [X] custom handler 지원하는지 확인 + https://github.com/deptno/nvim/blob/7fbcc6e28c113612d5d29dd7ca7057e87b3caeab/lua/lab/gx/init.lua#L21-L32
  - [X] *bug* visual selection 모드에 gx github 를 할 경우에 정체 경로가 전송됨 https://github.com/deptno/nvim/commit/e9cb8368620e2561a4ac5d5826b8bfb9f3de68b2
  - [ ] *feat* image open
  - [X] *feat* vV 번역 + https://github.com/deptno/nvim/commit/8f847e6
  - [X] *feat* vV search + https://github.com/deptno/nvim/commit/d55c9c2
  - [X] *feat* n search + https://github.com/deptno/nvim/commit/a25066f
  - [X] *feat* n find files + https://github.com/deptno/nvim/commit/aeae45c
  - [X] *bug* vV github permalink 에서 Line 을 못 얻어 옴, 어느 순간 부터 인지 + https://github.com/deptno/nvim/commit/b253387
    - multiple gx 에서 선택하는 수간 mode가 normal 로 인식되면서 생기는 문제
  - [ ] *feat* vV github permalink 를 가져올대 현재 커밋이 아닌 해당 영역 log들의 순서상 최신으로 하는 것이 좋아보임
  - [X] *bug* vV papago 깨짐
  - [ ] *feat* filetype 별 gx enable 여부
  - [X] *feat* node_modules 내에서 하면 해당 라이브러리 기준으로 처리가 필요, pnp
    - pnp 까지 처리하려고했으나 .git 정보가 없어서 커밋정보에 접근하지 못하기 때문에 의미가 약함  레포지터리까지만 여는 것으로 멈춤 + https://github.com/deptno/nvim/commit/b5395499a00a0e4e1dab68bd50032d469f3381b5

## link
- [[neovim]]
- [[neovim-lua-plugins]]
- [[vim-session]]
