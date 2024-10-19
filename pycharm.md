# pycharm
## [[jupyter-notebook]]

### 로컬 + [[pipenv]]
- 로컬에서 [[pipenv]] 환경을 만든다
- 해당 폴더를 프로젝트로 연다
- interpreter 는 [[pipenv]] 로 설정한다
  - 다른 프로젝트에서 미리 연결해놓은 가상환경을 사용할 수도 있지만 **그럴일 없음**
  - python 버전이 여러개인 경우 선택하면 된다
  - 선택시에 알아서 가상환경이 생성된다

### ssh remote 개발 환경
#### ssh 접속 후 가상개발 환경설정
- [[terminal]] 에서 [[ssh]] 로 접속해서 프로젝트 폴더 생성
- [[pipenv]] 를 사용해서 개발 환경 생성
```sh 
pipenv --python 3.10
```

#### pycharm 에서 접속 사용
- 프로젝트 생성 -> [[ssh]] 로 접속해서 프로젝트 폴더 생성
- python interpreter 설정을 [[pipenv]] 로 설정

### trouble shotting
- vim binding bug, 이동하다보면 스크롤 위아래로 튐
  + https://youtrack.jetbrains.com/issue/VIM-2501/IdeaVim-scrolls-when-typing-in-RMarkdown-with-Plots-DataSpell
  - `scrolloff=0` 설정하라는 말
## link
- [[intellij]]
- [[pipenv]]
- [[jetbrains]]
- [[jupyter]]
