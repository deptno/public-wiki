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

## % 특수 명령어
```sh 
%time [code] # 소요 시간 표시
%%time # 이건 셀 가장 윗줄에 사용
%%timeit # 이것도 소요시간 표시인데 차이점 파악 필요
%pip install [package-name]
```

## cli
```sh 
jupyter kernelspec list # 등록 된 커널 리스트
```

## 개념
- kernel
  - jupyter 에서 code block 을 실행하기 위한 주체
    - `jupyter kernelspec list` 를 통해 확인된 디렉토리에 `kernel.json` 파일을 출력해보면 언어와 해당 언어의 인터프리터 위치가 연결된 것을 확인할 수 있다.
  - 질문
    - [X] 왜 [[pycharm]] 은 interpreter 를 통해 python 환경을 강제하나
      - 아마도 jupyter notebook 이 아닌 순수 python 을 위한 용도로 생각됨
      - 아마도 jupyter notebook 에서 설치한 패키지와 일반 파이썬 파일에서 사용되는 패키지위치는 서로 다를 것
    - code block 에서 shell command 사용의 문제점
      - `!pip install ...`, `%pip install ...` 등이 혼용되어 사용되고 있는데 차이점이 있다.
      - `!` 는 jupyter 의 실행 환경인 파이썬의 범주에 설치된다
        - jupyter 의 실행환경과 kernel 의 인터프리터(python) 이 다르면 다른 곳에 패키지가 설치된다
      - `%` 는 kernel 에서 사용하는 파이썬의 범주에 설치된다
      ```mermaid
      flowchart
        subgraph global
          direction LR
          subgraph env0
            python0
            package0
            package0 o-.-o jupyter
          end
          subgraph env1
            python1
            package1
          end
          subgraph env2
            python2
            package2
          end
          subgraph kernel1
            subgraph c1[code block 1]
              %1[%pip install PACKAGE_NAME]
              !1[!pip install PACKAGE_NAME]
            end
          end
          subgraph kernel2
            subgraph c2[code block 2]
              %2[%pip install PACKAGE_NAME]
              !2[!pip install PACKAGE_NAME]
            end
          end
          python1 -.-> kernel1
          python2 -.-> kernel2
          jupyter -.-> kernel1
          jupyter -.-> kernel2
          python0 --> jupyter
          %1 --> package1
          %2 --> package2
          !1 ==> package0
          !2 ==> package0
        end
      ```
      - 중요한 차이는 jupyter kernel 이 `jupyter` 의 실행환경과 일치하는 인터프리터를 사용하지 않을 경우 `%`, `!` 사용에 따라 패키지를 불러오지 못하게된다

## git
- [[git-diff]] 를 위해서 `nbdime`

## link
- [[ai]]
- [[python]]
- [[pycharm]]
- [[huggingface]]
