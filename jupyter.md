# jupyter

## 설치
```sh 
pip install jupyterlab
# 커널 생성
pip install ipykernel
```
> [[pycharm]] 에서 쥬피터 노트북을 실행시에는 `jupyter` 의 설치가 필요

### lsp
```sh 
# lsp
pip install ipython jupyterlab-lsp python-lsp-server
```
- 결과적으로 잘 안됨, [[pycharm]] 하고 격차가 너무 크다

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
# 등록 된 커널 리스트
jupyter kernelspec list

# 커널 등록, 현재 파이썬 실행환경으로 커널 생성
python -m ipykernel install --user --name=KERNEL_NAME --display-name "KERNEL DISPLAY NAME"

# 커널 실행
jupyter kernel --kernel=KERNEL_NAME
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
      flowchart RL
        subgraph global
          python0[python *system*]
          subgraph package0[global packages]
            jupyter[jupyter *runtime*]
          end
          subgraph kernel0[ipykernel]
            subgraph c0[code block 0]
              %0[%pip install PACKAGE_NAME]
              !0[!pip install PACKAGE_NAME]
            end
          end
        end
        subgraph j[jupyter *runtime*]
          subgraph virtual env 1
            subgraph kernel1[ipykernel]
              subgraph c1[code block 1]
                %1[%pip install PACKAGE_NAME]
                !1[!pip install PACKAGE_NAME]
              end
            end
            subgraph package1[packages]
              jupyter1[jupyter]
            end
          end
          subgraph viertual env 2
            subgraph kernel2[ipykernel]
              subgraph c2[code block 2]
                %2[%pip install PACKAGE_NAME]
                !2[!pip install PACKAGE_NAME]
              end
            end
            subgraph package2[packages]
              jupyter2[jupyter]
            end
          end
          jupyter1 -.ipykernel install.-> kernel1
          jupyter2 -.ipykernel install.-> kernel2
          %0 --> package0
          %1 --> package1
          %2 --> package2
          !0 ==> package0
          !1 ==> package0
          !2 ==> package0
        end
      ```
      - 중요한 차이는 jupyter kernel 이 `jupyter` 의 실행환경과 일치하는 인터프리터를 사용하지 않을 경우 `%`, `!` 사용에 따라 패키지를 불러오지 못하게된다
      - viretual env 자체는 jupyter 안에 있는 것은 **아니다**
      - 다만 kernel과 연결되어 있다

## git
- [[git-diff]] 를 위해서 `nbdime`

## plugin
### :nbconvert:
#### `py` 파일로 변환
```sh
# `py` 파일로 변환
jupyter nbconvert --to file.ipynb
````

#### 특정 셀을 제외하고 스크립트 변환을 할 수 있다.
- jupyter notebook 파일에서 cell에 태그 입력, 예제 기준으로는 `remove` 삽입
- 컨피그 생성
  ```sh
  ## nbconvert 를 위한 config 생성
  jupyter nbconvert --generate-config
  ```
- `~/.jupyter/jupyter_nbconvert_config.py` 파일에 아래 내용 추가 후 스크립트 빌드
  ```
  c.Exporter.preprocessors = ['nbconvert.preprocessors.TagRemovePreprocessor']
  c.TagRemovePreprocessor.remove_cell_tags = {"remove"}
  ```

### jupytext
- plugin 인지는 모르겠으나 jupyter notebook 과 py 를 sync 로 생성하여 관려가 가능한 것으로 보인다

## link
- [[ai]]
- [[python]]
- [[pycharm]]
- [[huggingface]]
