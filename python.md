# python

## 환경생성
- [[pipenv]] 참조

## 언어 특성
- 참
```python 
True == 1
False == 0
1 and 1 == True
1 and 2 == True
1 & 2 == False
```

## 문법
### import
```python
import module
import module as m
from module import *
from module import sub_module
from module.sub_module import *
from module import func as f
```
### try - catch
- 아래 4가지 구성으로 되어있음
  - try
  - except
  - else
    - except 가 나지 않은 경우
  - finally

## 리스트 컴프리헨션
- for-loop 보다 빠르다

## 데이터 타입
### string
- `"""` + `\` 이스케이프 문자를 섞어서 첫줄이나 마지막줄에 원치 않는 빈줄 제거
```python
a = """\
a
b\
"""
```
- 출력
```
a
b
```

### tuple
- 불변
```python
tuple1 = (2.3,) # , 를 추가해야지 튜플로 인식
float1 = (2.3) # 없는 경우 float 으로 처리됨 type(float1) 을 통해 확인
tuple2 = 1,2,3,4,5 # 괄호가 없어서 튜플로 생성됨
```

### boolean
- 참 거짓 구분, 리스트,문자열,튜플,딕셔너리 등은 값이 비어있는지 여부를 가지고 판별

## 라이브러리
- [[numpy]]
  - c로 구현
  - list vs ndarray
    - ndarray 는 타입이 하나로 통일되어야한다
- pandas
  - c로 구현

## link
- [[pipenv]]
- [[ai]]
- [[openai-whisper]]
- [[ubuntu]]
- [[huggingface]]
- [[jupyter]]
- [[pyenv]]
- [[venv]]
- [[numpy]]
- [[pandas]]
