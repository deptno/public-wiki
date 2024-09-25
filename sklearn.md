# scikit learn
- `sklearn`

```python
from sklearn.impute import SimpleImuter
from sklearn.impute import KNNImputer
from sklearn.impute import MissingIdicator

# 결측치를 채우기 위한 imputer 생성
imp = SimpleImputer(
  missing_values=np.nan, # `np.nan` 을
  strategy='constant', # 특정 값으로 바꾼다
  fill_value=-9999, # 특정 값은 `-9999`, 문자열 보간도 가능
)
# strategy 에 `'most_frequent'` 를 주게되면 `df.mean()` 과 같은 효과

imputed = imp.fit_transform(df2.loc[:, ['age']].values)
imputed_df = pd.DataFrame(imputed, columns=['age'])

# 단순화하면
imputed_df = imp.fit_transform(df2.loc[:, ['age']])

# 평균 결측치 처리
imp = SimpleImputer(
  missing_values=np.nan, # `np.nan` 을
  strategy='mean', # 평균 값으로 바꾼다
)

# knn 을 이용해서 가장 가까운 애들 기준 보간
imp = KNNImputer(
  n_neighbors=2,
  weights='uniform',
)

```

## link
- [[ai]]
- [[python]]
- [[numpy]]
- [[pandas]]
