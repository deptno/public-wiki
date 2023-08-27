# pipenv

- virtualenv 등을 썼던 것으로 기억하는데 현재는 pipenv 로 가상환경이 추천된다한다
  + https://heytech.tistory.com/320
- 프로젝트별 환경 격리를 위해 존재

## 설치
```sh
pip install pipenv
```

## 가상환경 생성
```sh
mkdir project_name
cd project_name
pipenv --python 3.10 # 사용하고자하는버전
pipenv shell # 혹은 pipenv shell --python 3.10
exit # 가상환경 나가기
pipenv --rm # 가상환경 삭제
```

## link
- [[python]]
