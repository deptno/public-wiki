# jupyter

## 설치
```sh 
pip install jupyterlab
```
> [[pycharm]] 에서 쥬피터 노트북을 실행시에는 `jupyter` 의 설치가 필요

## 실행
```sh 
jupyter lab [--ip 0.0.0.0]
# 노트북으로 실행
jupyter notebook
```
- 외부에서 접속 필요시 옵션 추가 `--ip 0.0.0.0` 대입

## 소스 컨트롤
```python
doc([FUCTION_NAME]) # 함수 문서

?[FUCTION_NAME] # 함수 시그니처, 위치 정보 및 문서
[FUCTION_NAME]? # 동일

%psource [FUCTION_NAME] # 함수의 정의 코드로 보여줌

??[FUCTION_NAME] # ? + %psource
FUCTION_NAME]?? # 동일
```


## hash
```sh 
#|export_exp [name] # export 파일 확장자
#|export # 코드 익스포트시 포함되는 셀
```

## %
```sh 
%time [code] # 소요 시간 표시
%%time # 이건 셀 가장 윗줄에 사용
```

## link
- [[ai]]
- [[python]]
- [[pycharm]]
- [[huggingface]]
