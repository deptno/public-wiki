# python

## 환경생성
- [[pipenv]] 참조

## syntasx
### import
```python
import module
import module as m
from module import *
from module import sub_module
from module.sub_module import *
from module import func as f
```

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

## link
- [[pipenv]]
- [[openai-whisper]]
- [[ubuntu]]
- [[huggingface]]
- [[jupyter]]
- [[pyenv]]
- [[venv]]
