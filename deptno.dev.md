# deptno.dev
> 보조 기억 장치
> github 에 존재하는 wiki를 쓰고 있었는데 github 에서 wiki를 버렸다 생각될 정도로 구성이 수상하다. 일단 이를 깃 레포지터리로 이용하는 것은 유지하되, 편하게 접근하고 읽을 수 있도록 시스템이 필요했다.

## [[@todo]]
- [X] 정적 배포시에도 [[meilisearch]] 는 업데이트되어야함 [[diary:2023-12-30]]
  - [ ] [[github]] page  배포에서는 `.` 을 파일 확장자로 구분해서 패스에 있는 경우 문제가 됨, md 내에서 교체 예정
  - [ ] 보안상 로컬 크론잡 필요할듯
  - [ ] 이번에 search-console 요청 실패 여부 보고 custom url 적용여부 결정
  - [X] key 관리필요
    + https://github.com/deptno/deptno.dev/commit/8832cb92aa0ab3cdf87432a4b3eaf2689741b2ab
- [X] 정적 배포 지원, 집 서버에 구글 봇이 잘 못들어오는 거 같아서 seo 탄력을 못받음, ISP 쪽에서 괴롭히는건가 싶어 static 빌드 지원, 서버띄울 필요도 없긴함 사실
- [X] about me 추가, 해당 파일을 특수 파일로 인식하도록
- [X] [[mermaid]] 지원 추가 + https://github.com/deptno/deptno.dev/commit/f309f036802488dbfa048310d8da4c4b39679be5
- [X] diary next, prev 이동
- [X] 개발 환경에서 graph 클릭시 live 버전 링크가 걸려있음 + https://github.com/deptno/deptno.dev/commit/8866a2959a6c195162b225f47ef71412fa28309a
- [X] markdown 기본 포맷에서는 `-` 가 공백으로 변환되는 것으로 보인다 [[shell-script]] 추적해서 확인 + https://github.com/deptno/deptno.dev/commit/23bfff89604ecc93f4145a0bb9614a1fe8245a1f
- [X] 배포 버전에서는 최근 수정파일이 시간순이 아님, clone 받아서 그런것으로 추정 + https://github.com/deptno/deptno.dev/commit/20a39fa2b7ddb8c8c681646a0a3674f1116bb8e8
- [X] `[]()`의 링크 형태에 대해서 markdown 은 제대로 파싱되나 백링크등 데이터를 만들어내지 못함
- [X] 최근 수정된 파일 목록 + https://github.com/deptno/deptno.dev/commit/efe783c94b4e387f35819c5c304369d25b046ced
- [X] `diary:` 등의 prefix 처리 -> ~~해보니 이미 처리했었나봄~~ 글이아닌 태그와 그래프에서 이동시 문제가 있다
  - [X] `/` 로 시작하는 경로는 받아 들이지 못하는 것으로 보임, graph, tag + https://github.com/deptno/deptno.dev/commit/21b5cf1
  - [X] `diary:` 가 아닌 `diary/` 로 변경 + https://github.com/deptno/deptno.dev/commit/698c09d
    - [X] `wn.wikiname:file:` 형태 
  - [X] 링크 처리 + https://github.com/deptno/deptno.dev/commit/21b5cf1
    - [X] markdown renderer + link.ts 참조
    - [X] graph
    - [X] tag
- [X] frontend revision 노출 + https://github.com/deptno/deptno.dev/commit/6b3c35b
- [X] encoded uri 가 노출되는 문제, i.e. @todo -> %40todo + https://github.com/deptno/deptno.dev/commit/420d203
- [X] history, edit 기능이 wiki 와 달라서 처리 필요 + https://github.com/deptno/deptno.dev/commit/312682a
- [X] [[deptno.dev]] 에서 push event를 받아서 자체 재시작(업데이트가 아닌)하도록 설정
  - [X] process.exit + livenessProbe 로 process 를 재시작할 뿐 pod 나 container 를 재시작할 수 없음
  - [X] 결국 [[webhook]] -> [[kubernetes-api]] 를 통해 rollout 을 하는 방향으로 수정되어야함
  - [X] 생각해 보니 서버가 아닌 wiki 의 레포가 일반 레포가 아닌 wiki repo 여서 이벤트를 ~~받을 수 없음~~
    - 당분간 수동
    - [X] gollum webhook event 가 있어서 위키도 이벤트를 받을 수 있음
      + https://github.com/deptno/deptno.dev/commit/7f2d0cd8a65973b35476f131dfe13442ae468d04
- [X] https://github.com/deptno/deptno.github.io/wiki -> https://github.com/deptno/public_wiki 로 이사 [[diary:2023-10-14]] 
  - [X] rename directory
  - [X] git remote 변경
  - [X] vimwiki directory 설정 변경 + https://github.com/deptno/nvim/commit/ab2f2af6
  - [X] zsh alias ~~변경~~ -> 제거로 처리 + https://github.com/deptno/.config/commit/f0e915b
  - [X] deptno.dev 에서바라보는 directory 설정 변경 + https://github.com/deptno/deptno.dev/commit/2b5af1a
  - [X] github hook url 확인
    - [X] 추가 - https://github.com/deptno/public-wiki/settings/hooks -> 동작 확인됨
    - [X] 제거 - https://github.com/deptno/deptno.github.io/settings/hooks/419255020
  - [X] kubernetes cluster 수정 + https://github.com/deptno/cluster-amd64/commit/73a434d

## why
- 나이가 들면서 잊어버리는 속도가 점점 빨라진다.
- 아무도 관심 없어 하는 것을 혼자서 깊게 팔 수 있다. 이런 기록은 가치가 있다.
- 나라는 특성은 고유하므로 같은 행동을 나중에 또 할 확률이 높다.
- 내 기록이 나에게 도움이 된다.
- 기록도 중요하지만 **검색도 중요**하다.
- 노션은 느리다, 검색이 어렵다.
- github wiki 는 폴더 중첩을 허용하지 않는다. 이 부분이 문제가 되진 않지만 vimwiki 의 방식과 충돌하는 점이 있다.
- vimwiki 는 개인적 사용에서는 훌륭하지만 디바이스 한정적이며 gui 지원에 한계가 있다.
- 글의 생산을 위한 기기와 소비하는 기기는 다를 수 있음으로 로컬을 벗어나야한다.

## wiki vs blog
- blog 는 일종의 개인 sns로 보여진다. 남이 읽을 수 있도록 배려되어야한다.
- wiki 는 정보의 링크에 강점이 있다. 글을 작성하면서 해당 글에 마킹을 해두면 추후 해당 페이지의 상세가 작성되었을 때 링크는 자동적으로 동작한다.
- 남이 읽는 것을 전제하면 글을 작성하는 시간이 배 이상 소요된다.
- 빠르게 기록을 남기기 위한 시스템으로는 wiki 가 적합하다.
- 개발 기술들은 연관된 많은 기술들로 이루어지며 참조가 필요하다. 이 역시 wiki 가 적합하다.

## static vs server
- static 무제한 트래픽에 seo 부스팅을 받을 수 있으며 심지어 무료로 퍼블리싱이 가능하다.
- 서버는 무한한 확장성을 가진다. - 트래픽 외

## deptno.dev
가지고 있는 메인 도메인을 놀리고 있는 매번 하고 있는게 있다보니 채워넣지를 못했다. 아무래도 메인 도메인이기 때문에 나를 위한 것으로 채워 넣고 싶었는데, 이직 같은 커리어 패스나 이런 것에도 별 관심이 없다보니 타인을 위한 글 보다도, 나를 도울 수 있는 시스템을 설계해서 스스로를 강화하는데 쓰기로 했다.

최근 이직을 해서 좀더 매니징에 가까운 업무를 해보니 하나에 집중할 수 있는 여건은 없는 것 같고 산발적으로 들어오는 이슈 혹은 질문을 우선순위에 맞게 정리하고 있다가 데드라인이 지나기전에 처리해야하는 업무들이 많아졌다.  
원격 근무라는 새로운 환경, 새로운 업무에 적응하기도 힘들텐데 개인적인 이슈까지 겹치고 하다보니 오히려 기억을 못하는 단계에 접어들었고 살기위해 기록의 필요성을 느끼게 되었다.

이전부터 블로그와 같이 기록과 퍼블리싱에 대해서 관심이 많았는데 현재는 그런류의 기록이 아닌 실제 업무에서 살아남기 위한 기록을 위한 점이 이전 시스템들과 차이가 있다

## 요구사항
내가 생각하는 요구사항은 크게 아래와 같다.

- 블로그와 같이 기록과 퍼블리싱이 가능해 외부 접근이 가능해야한다.
- 위키와 같이 이어질 글이 없더라도 주제를 미리 이어놓을 수 있어야 한다.
- 글을 접근하기 위한 방식으로는 **시간**, **주제** 라는 두가지 방식으로 접근이 가능해야한다.
- 업무 처리를 위해서 우선순위를 관리하고 todo list 와 같은 형태의 처리를 지원해야한다.

그 중에서도 접근 방식에 대해서 정리하자면 3가지가 존재한다.
- 구조를 통한 접근, 폴더, 파일 구조
- 검색을 통한 접근, 키워드를 통한 검색을 지원하는 에버노트, 원 노트등이다
- 시간을 통한 접근, 내가 작성한 시간을 통해 접근가능해야한다. 파일 시스템은 기본적으로 수정시간을 가지고 있지만 시스템으로 이를 지원하는 경우는 보지 못한 것 같다.

## 시스템 비교
google keep, mac note, good note, evernote, [[orgmode]] 등을 사용해 보았는데 지금은 다 기억나지 않아 최근 것 위주로 표로 정리해보니 아래와 같았다.

| 기능\시스템      | vimwiki(+termianl) | blog(+github.io) | todo app | notion | task warrior |
|------------------|--------------------|------------------|----------|--------|--------------|
| 외부 접근        | x                  | o                | x        | o      | x            |
| 문서간 연결      | o                  | /                | x        | o      | x            |
| 키워드 검색      | o                  | /                | o        | o      | x            |
| 목록을 통한 접근 | o                  | o                | o        | o      | o            |
| 시간을 통한 접근 | /                  | o                | o        | x      | o            |
| 백링크           | o                  | /                | /        | o      | o            |
| 할일 목록 처리   | o                  | /                | o        | o      | o            |

### vimwiki
최근 2년 정도는 vimwiki 를 잘 써왔다. 이게 아쉬운 점은 로컬에서 [[git]] 으로 관리된다는 점인데 글을 쓰는 관점에서는 [[vim]] 을 기반으로 동작하다보니 타이핑에 대한 즐거움도 있고 그 wiki 기능도 동작을 잘해서 활용도가 높다. 로컬 내에서의 검색이기 때문에 검색도 매우 빠르기 때문에 장점이 많은데도 불구하고 gui 가 없어서 다이어그램등을 보기위한 추가 세팅이 필요한 점이 아쉽고 또 최종적으로 외부 접근이 안되니 가끔 누워서 뭘 보려고할 때 아쉬운 점이 있었다. github 의 wiki 기능을 활용하면 조금 더 지원을 받을 수 있는데. github wiki 의 경우에는 파일의 폴더구조를 지원하지 않아 한 파일명의 유일성이 보장되어야 정상 동작을 하기 때문에 이 이런 부분이

> 이 페이지 글은 stash 되어있는 것을 가져왔더니 글을 작성하다 만 상태였다

## 구현
또 위키 시스템도 많은데 개인적으로 vim을 쓰고 있으니 vimwiki 를 사용하면 된다. 많은 블로그 플랫폼들이 markdown 을 기반으로 하고 있으므로 아무거나 쓰면 되는데 직업이 프론트엔드 개발자이기도하고 완전한 커스터마이징을 하기 위해서 직접 구현하기로 했다.

## link
- [[project]]
- [[gtd]]
- [[taskwarrior]]
- [[webhook]]
