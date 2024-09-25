# numpy

## list vs ndarray
- `list` 에 비해 빠르다
- 하나의 타입만 지원한다
- `data[i,j]` 형태의 접근이 가능

### 사용
```python
# list 로 부터 생성
numpy.array(list_data, dtype)

# 생성
numpy.zeros()
numpy.ones()
numpy.full()
numpy.arrange(start, end, step)
numpy.linspace(start, end, division_n) # 등분 생성
numpy.random.random()
numpy.random.normal()

# 변경
numpy.reshape(-1, 5) # 차원 변경
```

### 유니버설 연산
- array 간 연산을 지원
- broadcast 을 지원해서 ndarray + scalar 는 scalar 가 ndarray 로 확장되서 연산

## link
- [[python]]
- [[pandas]]
- [[sklearn]]
