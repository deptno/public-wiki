# mlflow
- 튜닝 하이퍼파라메터 관리
  - 반복적인 작업이 많은 하이퍼파라메터등을 기록 및 시각해주면서 트래킹이 가능
- 모델
  - 모델에 등록하면 rest api 를 통해 inference 가능
- 모델 레지스트리 관리
  - 모델의 버전 관리 및 배포버전등에 대한 관리
- api 서빙은 flask 기반을 제공

## 설정
```sh 
python -m venv .venv
source .venv/bin/activate

pip install mlflow
```

## 사용
```python
import mlflow

mlflow.set_tracking_uri('http://localhost:5000')
# mlflow.get_tracking_uri()

exp = mlflow.set_exprement(experiment_name='name_of_exp')
# exp.name
# exp.experiment_id
# exp.artifact_location
# exp.create_time

mlflow.autolog()
mlflow.start_run()

# model 작업
model = LogisticRegression(max_iter=0)
model.fit(X_train, y_train)
pred = model.predict(X_test)
accuracy = accuracy_score(y_test, pred)

mlflow.end_run()
# 이후 ui에서 확인 가능
```

### `with` 절과 함게 사용, `end_run` 대체
```python
with mlflow.start_run():
  # model 작업
  model = LogisticRegression(max_iter=0)
  model.fit(X_train, y_train)
  pred = model.predict(X_test)
  accuracy = accuracy_score(y_test, pred)
```

## 운영
### 태깅
```python 
from mlflow.tracking import MlflowClient
client = MlflowClient()

client.set_model_version_tag(
  name='model_name',
  version='version',
  key='stage',
  value='production' # 형태로 현재 스테이지를 태깅하여 관리
)
```

## 실행
```sh 
mlflow ui
```

## 모델 서빙
- `./mlartifacts` 안에 `experiments` 데이터가 쌓여있음 여기서 찾아서 서빙
```sh 
mlflow models serve -m ./mlartifacts/599912536112484580/4b19152236224ce08ef46ffd1b6e72d6/artifacts/model -p 5001 --no-conda 
```

## link
- [[python]]
- [[ai]]
- [[venv]]
- [[conda]]
- [[jupyter]]
