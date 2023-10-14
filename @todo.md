# @todo

- [[neovim-setup]]

- [ ] pods 110 개 제한 풀기 (cidr 이슈)
- [ ] 거울 사기
- [ ] grafana [[alarm]]
- [X] ha 깨짐 [[home-assistant]]
  - [ ] ip_bans.yaml 파일에 왜 서버 자기 자신의 ip가 추가되는지 확인해서 조치 필요
- [X] fork 프로젝트 sync 타이밍에 overwrite 된 README 를 유지할 수 있는 방법 검토
  + https://github.com/deptno/nvim/blob/d049fc86fe19354ee0d3373b707fbc91fdf0b5e6/.github/README.md
  - .github/README.md 를 사용하면 overwrite 걱정 우려가 없고 repo 에서도 해당 방법으로 사용되고 있어서 해결됨
- [ ] 라즈베리파이 판매
- [ ] 요금제 찾아보기
- [ ] 에어컨 청소
- [o] [[tmux]]
  - [X] tmux pane 생성시 이전 pane 기준의 cwd 설정 https://github.com/deptno/.config/commit/5e85f87f
  - [ ] tmux-thumbs 색상 설정
- [X] vim 에서 cwd 혹은 현재 파일의 위치를 가지고 [[tmux]] {pane,window} 를 생성할 수 있도록 지원 - https://github.com/deptno/NvChad/commit/8e6dfa1
- [.] git terminal 확장
  - [ ] `.github/CODEOWNERS` 파일을 기준으로 fzf 자동완성 기능 구현
```sh 
cat .github/CODEOWNERS | grep -v '^#' | grep -E '@[A-z0-9_-]+' | awk '{ for (i=2; i<NF; i++) print $i;}' | sort | uniq 
```
  - [ ] graph 확인
  - [ ] mergetool [[intellij]] 이용
    - [ ] https://gist.github.com/ffittschen/6d9be1720f30eb8dc0142cc0ed91c7d9
  - [X] difftastic -> delta 로 변경, forgit 지원이 안되는 문제가 있어서 변경 https://github.com/deptno/.config/commit/e8ef36479716e41276efe3b6275f5806b9eaae97
      - [X] git show commit
    - 여러 커밋을 선택해서 변경된 파일 리스트 보기
      - 해당 파일의 diff
  - [ ] last commit diff 보기
  - [ ] 현재 브랜치와 부모 브랜치 사이의 diff
    - [ ] 현재 브랜치에만 포함된 커밋들로 확인
  - [ ] 현재 파일
    - [ ] 현재 파일에서의 수정사항 log
    - [ ] 현재 파일에서의 선택 영역 수정사항 log
  - [ ] log: preview navigation
- [X] 회사 chrome profile -> safari profile
- [ ] rust 해볼만 한 미션
  - [ ] https://github.com/will-stone/browserosaurus -> tauri 전환
  - [ ] https://bevyengine.org 튜토리얼 진행
- [ ] 모든 ui 화면에 공유한 이름이 있어서 wiki 에 찾아 갈 수 있어야한다
  - [ ] 카카오톡 알림톡 등도있음
  - [ ] web이면 url
  - [ ] app인 경우 navigation screen 이름
- [X] 특정 레포에서 라이브러리 정의를 못따라가는 경우가 있음
  - [X] 해보니 설정이 pnp모드라 이거 관련된 것인지 확인 필요
  - [X] 프로젝트 root 에서 `yarn dlx @yarnpkg/sdks vim` 설치 필요, `lbrayner/vim-rzip` 설치 필요
  + https://github.com/deptno/nvim/commit/ed738ae

- [ ] 2022-11-25 지라 백로그에서 스프린트 설정 배치 처리가 필요

## link 
- [[idea]]
- [[project]]
