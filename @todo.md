# @todo

- [ ] pods 110 개 제한 풀기 (cidr 이슈)
- [ ] 거울 사기
- [ ] grafana [[alarm]]
- [X] ha 깨짐 [[home-assistant]]
  - [ ] ip_bans.yaml 파일에 왜 서버 자기 자신의 ip가 추가되는지 확인해서 조치 필요
- [o] vim 설정
  - [X] vim cd.. 이 ~~cwd 기준이라 recursive 하게 동작하지 않는 문제~~ 수정
    - [X] vim cd.. cwd 기준이 아니라 현재 파일 기준이라 동작하지 않는 것으로 cwd 기준으로 수정 필요
  - [O] [[vim-startify]] 설정
    - [X] startify session 에서 cwd 지정이 가능한지 확인
      - [X] startify_change_to_dir  옵션을 사용해 봤으나 사용성이 떨어지고 git root 로 가야함 -> 범용적으로 cdp 구현 프로젝트로 루트 이동이 가능
        + https://github.com/deptno/NvChad/commit/a869b4a4
    - [X] tmux window -> session
    - [X] head text 제거
    - [X] git modified 와 untracked 를 합칠 것
    - [X] 나갈때 임시 세션(`_latest`)으로 저장
    - [X] vim-startify 설정에 대한 기록 작성
    - [O] 옵션 검토
      - [X] g:startify_update_oldfiles - vim 시작시점이 아니라 계속적인 업데이트로 이해
      - [X] g:startify_session_persistence - 이렇게 되면 세션이 최종 상태를 기억하게됨, _latest 가 번잡하면 이걸 테스트
      - [X] 마지막 session 간의 전환 지원이 필요 https://github.com/deptno/NvChad/commit/1996f7c
      - [X] ~~임시 세션 저장 슬롯 여러개 지원~~ - 의미 없을 것 같음
      - [X] g:startify_session_before_save - ~~nvim-tree~~, ~~symbol-outline~~ 등 제거, 둘다 문제가 있어서 제거 못함
        - https://github.com/deptno/NvChad/commit/cf9c4a9
      - [X] cmd 지정이 가능함 - 마지막 세션간 전환 구현
      - [X] ~~session 에 속한 파일이 열리는 순간 해당 session 을 여는 기능 검토(이걸 쓰면 `_latest` 는 드랍)~~
      - [ ] repo 의 경우 branch 별 세션 저장 및 변경 방법 고민
    - [ ] session bookmark 가 필요한것
  - [.] [[gx]]
    - [ ] search 시에 ' ' -> '+' 로 나가는 이슈 처리
    - [ ] custom handler 지원하는지 확인
    - [ ] 하나의 패턴에 대한 handler 선택 가능 여부 확인
    - [X] gx extension ~~이건 gx.nvim 과 관련이 없는 개별 플러그인으로 보임
    - [ ] visual 모드에 진입시 gx 가능 영역들에 대한 하이라이트
  - [X] [[nvchad]] default theme 변경
  - [.] [[vimwiki]]
    - [ ] diary 를 폴더구조거아니라 prefix 형태로 평탄화 가능한 옵션이 있는지 검토
    - [ ] 회사와 private wiki 를 분리
      - [ ] 마이그레이션 - diary 를 통째로 복사하고([[git-subtree]]) 수동으로 필터([[git-rebase]])링해서 필터링할 것
    - [ ] *bug* 특정 케이스에 vimwiki가 꺼지는 상황이 있음 현재 파일 기준 24라인 이후에 뎁스가 더 들어간 todo를 생성하며 꺼짐
    - [X] timestamp, 혹은 다이어키를 삽입하는 키맵 추가 - 이미 snippet 자동완성으로 지원되고 있었음 `diso`
  - [ ] [[octo]] 도입 검토
  - [ ] [[neogit]] git checkout 대신 git switch 를 이용할 방법
  - [ ] diff 가 [[intellij]] 대비 불편함
    - [ ] diffview 가 활성화되면 theme 자체를 변경해버리는 것도 고려
    - [ ] change base 쉽게
  - [ ] buffer 최대 가로사이즈로 window width 설정
- [ ] fork 프로젝트 sync 타이밍에 overwrite 된 README 를 유지할 수 있는 방법 검토
- [ ] 받기 - https://www.clien.net/service/board/jirum/18342130?od=T31&po=0&category=0&groupCd=
- [ ] 라즈베리파이 판매
- [ ] 요금제 찾아보기
- [ ] 에어컨 청소
- [ ] *overdue* *2023-10-05* 주간 보고 작성 
- [X] tmux pane 생성시 이전 pane 기준의 cwd 설정 https://github.com/deptno/.config/commit/5e85f87f

---

- [ ] 2022-11-25 지라 백로그에서 스프린트 설정 배치 처리가 필요

## link 
- [[idea]]
- [[project]]
