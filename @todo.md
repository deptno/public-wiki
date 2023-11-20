# @todo

- [[neovim-setup]]

## 문서 정리
- [ ] [[vim]] [[regexp]]
  - very magic
  - \k
  - (a|b)
  - non-greedy
- [X] [[dynamodb]] 오래전에 정리하지 못한 것 기록

## 클러스터
- [ ] pods 110 개 제한 풀기 (cidr 이슈)
- [ ] grafana alarm ha 깨짐 [[home-assistant]]
  - [ ] ip_bans.yaml 파일에 왜 서버 자기 자신의 ip가 추가되는지 확인해서 조치 필요
- [ ] kubernetes certificate 갱신

## news
- [ ] openai
- [ ] copilot
- [X] feconf [[diary:2023-11-09]]
- [X] next 14 문서 읽기

## 일상
- [ ] 거울 사기
- [X] 주방 조명
- [ ] 라즈베리파이 판매
- [X] 요금제 찾아보기
- [X] 에어컨 청소
- [X] mifare classic tool 에서 현관키 못 읽는데 key range  찾아볼 것
  + https://www.icaria.de/mct/help-and-info/#read_tag
  - 해보니 key  값이 기본적으로 포함된 레인지에는 없는 것 같고 따로 찾을 방법도  없는 것  같다  중국산 실물 기기에 키 값이 많다는데 이 것도 복불복일것으로 예상 mct@4.1.0 에서 테스트
## 프로젝트 관리
- [X] fork 프로젝트 sync 타이밍에 overwrite 된 README 를 유지할 수 있는 방법 검토
  + https://github.com/deptno/nvim/blob/d049fc86fe19354ee0d3373b707fbc91fdf0b5e6/.github/README.md
  - .github/README.md 를 사용하면 overwrite 걱정 우려가 없고 repo 에서도 해당 방법으로 사용되고 있어서 해결됨
- [ ] [[comparison:intellij-vs-neovim]] lsp 기능 비교
- [ ] production 에 머지된 최근 브랜치가 master 에도 들어와있는지 확인

## 맥
- [X] 회사 chrome profile -> [[safari]] profile
- [X] 링크별 브라우저 선택
  1. [[finicky]] {safari,chrome-profile}
  2. safari-profile, 설정에 있음
- [X] [[bruno]] 설치 테스트 rest-nvim 이 불안정해서 대체 할지 검토
- [ ] [[finicky]] 에서 proxy 한 링크를 모두 기록해서 fuzzy 로 사용

## 터미널
- [ ] [[gh]] 설정 고도화
- [X] jira 티켓 생성등 템플릿화 -> 생성하고보니 {assign,watch,edit} 등이 더 가치있어보임
- [X] [[alacritty]] 세션 이동시에 디렉토리 접근 권한 묻지 않도록
  - [X] *fail* [[codesign]] 처리
  - [X] `전체 디스크 접근 권한` 을 줘서 해결
- [X] fx 사용 + https://github.com/antonmedv/fx
- [o] [[tmux]]
  - [X] tmux pane 생성시 이전 pane 기준의 cwd 설정 https://github.com/deptno/.config/commit/5e85f87f
  - [ ] tmux-thumbs 색상 설정
- [.] [[git]] terminal 확장
  - [ ] 해당 시점 이후의 브랜치목록
  - [ ] author 별 브랜치 목록, 시간 역순
  - [X] `.github/CODEOWNERS` 파일을 기준으로 fzf 자동완성 기능 구현 -> 불필요, cmp_git 으로 대체 가능
  - [X] graph 확인 -> [[lazygit]]
  - [ ] mergetool [[intellij]] 이용
    + https://www.jetbrains.com/help/webstorm/working-with-the-ide-features-from-command-line.html#arguments
    - [ ] https://gist.github.com/ffittschen/6d9be1720f30eb8dc0142cc0ed91c7d9
  - [X] difftastic -> delta 로 변경, forgit 지원이 안되는 문제가 있어서 변경 https://github.com/deptno/.config/commit/e8ef36479716e41276efe3b6275f5806b9eaae97
      - [X] git show commit
  - [.] diff
    - [ ] difftool 로 [[intellij]] 적용
    - [X] delta 적용 + `--ignore-all-space`
- [X] 특정 레포에서 라이브러리 정의를 못따라가는 경우가 있음
  - [X] 해보니 설정이 pnp모드라 이거 관련된 것인지 확인 필요
  - [X] 프로젝트 root 에서 `yarn dlx @yarnpkg/sdks vim` 설치 필요, `lbrayner/vim-rzip` 설치 필요
  + https://github.com/deptno/nvim/commit/ed738ae

## 러스트
- [ ] rust 해볼만 한 미션
  - [ ] https://github.com/will-stone/browserosaurus -> tauri 전환
  - [ ] https://bevyengine.org 튜토리얼 진행

## 업무
- [ ] 모든 ui 화면에 공유한 이름이 있어서 wiki 에 찾아 갈 수 있어야한다
  - [ ] 카카오톡 알림톡 등도있음
  - [ ] web이면 url
  - [ ] app인 경우 navigation screen 이름
- [X] jira 생성 템플릿 작성
  - [X] `.jira` filetype 정의 syntax: [[markdown]]
  - [X] sh command fence block + markdown content block 구성
    - [X] [[tree-sitter]] 로 분리
- [ ] rn 로그에 [[tailspin]] 적용
- [ ] 회사 프로그램에 릭있음 하던일만 뺄것

## link 
- [[idea]]
- [[project]]
