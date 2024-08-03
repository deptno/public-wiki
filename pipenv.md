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

> 공식문서 보지 않고 런바이 두잉
- `pipenv install [PACKAGE]` 패키지를 설치하고 `Pipfile` 에 기록된다
  - `pipenv shell` 활성화가 필요하진 않다
  - `pipenv install -U [PACKAGE]` 은 지원되지 않는 것으로 보인다

### `pipenv shell`
- `shell` 을 활성화 한다 하더라도 `pip` 를 통해 설치하면 `Pipfile` 에 기록되지 않는다 다만 격리된 환경에 인스톨된다

### `requirements.txt` 설치
```sh 
pipenv run pip install -r requirements.txt
```

## link
- [[python]]
- [[ubuntu]]
- [[stable-diffusion]]
- [[pip]]
