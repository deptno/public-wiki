# orgmode

- https://github.com/nvim-orgmode/orgmode

emacs orgmode 클론

```orgmode
#+TITLE: 제목

* TODO todo list
SCHEDULED: :now:
  - [ ] buy something
```

## shortcut
```lua
localleader = ;
```
- `g?`        단축키 목록 - `.org` 에서만
- `;oa`       아젠다 메뉴
- `;oc`       캡쳐 템플릿
- `C-a`       날짜 증가
- `C-x`       날짜 감소
- `C-Space`   로 체크박스 토글
- `;oA`       :ARCHIVE: 태그처리
- `;ot`       태그 삽입
- `;or`       서브트리를 포함하여 해당 파일로 전송 `.org` 포함해서 입력 필요

### in refile.org
1. refile.org 진입
- `;oc` -> `t`: Task
- refile.org 열기
2. template 작성후 `;ocr` -> `filename.org` 입력하면 해당 파일로 전송됨

### in agenda view
- `J`         캘린더 열기 
- `f`         다음 기간으로 이동
- `b`         이전 기간으로 이동

## abbreviations
:now: `Space` 로 현재 날자 삽입


## link
- [[neovim]]
- [[neovim-lua-plugins]]
