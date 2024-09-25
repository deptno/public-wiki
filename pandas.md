# pandas

## Series
- 벡터와 대응
- 2개(key, value)의 리스트로 생성
- dict 로 생성
- [[numpy]] 변환
```python
series.values # ndarray
series.to_list() # list
series.to_dict() # dict
```
- method
```python
series.drop_duplicates(column_name, keep='first') 
series.unique()
series.value_counts() # 유일값의 빈도
```

## DataFrame
- 행렬과 대응
- dict[key: string, value: Series]
- 결측치 보간 내용은 [[sklearn]] 참고
- merge
```python
pd.merge() # 열 이어붙이기
pd.concat() # 행 이어붙이기, 열 단위도 가능
df.corr() # correlation
df.info() # 데이터 타입, null 여부, 메모리 사용 양
df.values
df.copy()
df.isnull().sum()
df.isnull().any(axis=1) # 결측치를 가진 행들, True 행렬
df.dropna(axis=0) # 결측치를 가진 행 제거
df.mean() # 평균값
df.mode() # 최빈값

df.loc[:, ['age']].fillna(df.mean()[['age']])
df.loc[:, ['gender']].fillna(df.mode().iloc[0])

# 결측치 보간은 [[sklearn]] 참고

df['date'].dtype # dtype('O'), object, string 이란말
df['datetime'] = pd.to_datetime(df['date'])
df['datetime'][0].dayofweek
df['datetime'][0].weekofyear
df['dayofweek'] = df['datetime'].apply(lambda x: x.dayofweek)

# datetime 을용하면 `date_range` 함수 활용 가능
pd.date_range(start=df['datetime'].min(), end=df['datetime'].max(), frew='D')
```

## indexer
- loc
  ```sh 
  df.loc[df['column_one'] == 'R', ['column_name']]
- iloc

## column 선택
```python
df['column']
df.column
```
- 위와 같이 컬럼명과 속성이 겹치는 경우 각각대로 동작하므로 데이터를 원할대는 dict 형태의 접근 한다


## link
- [[python]]
- [[numpy]]
- [[sklearn]]
