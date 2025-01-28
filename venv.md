# venv
> local 환경을 생성하는데 폴더명을 명시하는 구조이기 때문에 같은 프로젝트에서 여러 환경지원도 가능할 듯
> 파이썬 기본 모듈이라는 장점

## 가상 환경 구축
```sh 
python -m venv .venv
source .venv/bin/activate
```

## exit
```sh 
deactivate
```

## [[error]]
- [[pip]] 가 `.venv` 에 없는 경우
```sh
# which python 으로 .venv 의 python 이 활성화 된 것 확인
python -m ensurepip --upgrade
which pip3
```

## link
- [[python]]
- [[pipenv]]
- [[conda]]
- [[pip]]
- [[poetry]]
