# pycharm
> [[jupyter]] notebook 기준

## 로컬 개발 + remote 실행 환경
- 로컬에서 환경을 만들고 remote 에서 실행한다 interpreter 는 [[pipenv]] 로 설정한다
- 해당 환경으로 프로젝트를 생성하면 

## ssh remote 개발 환경
### ssh 접속 후 가상개발 환경설정
- [[terminal]] 에서 [[ssh]] 로 접속해서 프로젝트 폴더 생성
- [[pipenv]] 를 사용해서 개발 환경 생성
```sh 
pipenv --python 3.10
```

### pycharm 에서 접속 사용
- 프로젝트 생성 -> [[ssh]] 로 접속해서 프로젝트 폴더 생성
- python interpreter 설정을 [[pipenv]] 로 설정

## link
- [[intellij]]
- [[pipenv]]
