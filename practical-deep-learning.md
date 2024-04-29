# practical-deep-learning
+ https://course.fast.ai
+ https://github.com/deptno/study-ai

## 강의
### 01. getting started
- 이미지 분석기 따라 생성해보기
- imagenet 을 학습한 기본 모델, resnet18 기반 개/고양이이를 추가학습(파인튜닝)

### 02. deployment
- huggingface 에 gradio 등을 통한 app + api endpoint 배포
- Image Classifier Cleaner, 학습시 잘못된 데이터 제거
- Data augmentation -> 데이터 증강, 이미지를 랜덤 사이즈 크롭, 혹은 변형을 통해 학습을 증대시킨다
- leaner 를 pickle 파일로 export -> import 사용

### 03. nautral net foundations
- loss 를 개선하는 원리 설명
- ReLU (Rectificed Linear Unit)
  - 0 이상에서만 값을 가짐
  - 큰수의 ReLU 계산을 행렬곱셈을 통해 가능(GPU 가 잘하는)
- Gradient descent
- Excel 의 spreadsheet 을 가지고 kaggle 문제를 수동으로 푸는 것 보여줌
  - 데이터 정규화
    - 나이가 없는 것을 제거
    - 승선지가 없는 경우 제거
    - 남은 데이터 컬럼(인자)들에 계수를 곱해야한다 -> 수치화 되어야함
      - 남/여 -> isMail(1/0)
      - 승선 -> [어디에] 승선 컬럼으로 컬럼 분화 -> 1/0(이진 변수화)
        - a/b/c 승선지인경우
        - isA, isB, isC 는 isA, isB 가 모두 0일때 확정되므로 필요없다
          - n-1 개의 dummy variable
    - `y = mx + b` 와 같이 방정식에는 상수항이있는데 이는 컬럼을 하나 만들어서 1로 채워서사용(랜덤 계수곱을 위해)
    - 각 인자들에 곱할 랜덤 계수들을 만듬(-0.5 ~ 0.5)
    - 나이 -> 맥시멈값을 가지고 각 컬럼값을 나눠서 0 ~ 1 로 정규화한다
    - fare(티켓값) -> 큰숫자가 많고 작은숫자가 조금 있는 경우, 극단적 데이터의 경우는 로그를 취한다

### 04. natural language (NLP)

## 책
### 04
- 파이토치 텐서
  + https://wikidocs.net/52460

- 평가 함수, 좋은지 여부를 사람이 확인하기 위해 사용, high is better
- loss 함수, 손실 최적화를 위해 학습에 사용됨, low is better
  - loss 가 낮을 수록 좋다
    - loss 가 낮다, 확신을 갖고 맞춘 경우
    - loss 가 높다, 확신을 갖고 틀린 경우, 맞췄으나 확신이 없는 경우
  - loss 함수로 **mse(mean sequare error)** 사용시
    - `평가할 데이터 - 정답` 을 제곱한뒤 평균을 내서 루트하는 방식
    - 평가데이터가 정답과 얼마나 근접한지 확인
- loss 함수의 결과를 미분하면 학습 방향을 알 수 있음
- 미분 결과 * learning rate 를 파라메터에서 뺀다
  - 기울기가 낮아지는 방향(0) 을 향해 전진
  - learning rate 를 찾는 것은 또다른 중요한 일, 여기선 낮은 값(0.001)정도로 생각
  - 다음 train set + 수정된 파라메터를 가지고 다시 loss 함수를 사용
  - 반복해서 낮은 loss 획득
  - parameter 를 모델 실행 후 결과에서 `backward()`시 parameter에서 미분된 값을 `grad` 속성을 통해 접근가능
    - 가중치가 벡터인 경우 `.sum()` 후 `backward()`실행으로 미분

#### 정리
- model 을 함수 f로 보고 모델은 입력을 받아 결과를 예측한다
  - 뭔 받아서 뭘 예측할 건지가 f
- loss 함수, f 의 결과가 답에 얼마나 부합하는지 확인한다
  - 답은 label data 에서 확인
- 임의의 파라메터를 설정하고 f 의 결과를 구하고 답과의 차이(거리)인 loss 구한다
- loss 를 미분(`backward` 함수가 여기있기 때문에 이렇게 표현) 파라메터의 기울구한다
- 파라메터의 기울기에 learning rate 를 곱하여 파라메터에 더하거나 빼서 더 낮은 loss 결과를 얻도록 한다
- 반복한다

#### 4.4.3 SGD
- f: 모델, 속도를 측정하고자함
- loss: 모델의 속도가 실제 속도차를 비교
- 속도 측정 흐름
  - x: time, y: speed, (a,y: weights, y: bias): parameter
  - 속도가 느려지다가 빨라진다 -> 2차방정식 `a(x**2) + bx + y`
  - loss가 적은 parameter 를 찾는게 목표, x, y 는 주어짐
  - random parameter 로 시작
  - 
#### 용어
- 브로드캐스팅: tensor 간의 연산, 높은 랭크의 텐서에 맞춰서 낮은 랭크의 텐서가 확장된다
- 파라메터: weights + bias

#### numpy vs pytorch
| numpy    | pytorch          |
|----------|------------------|
| 제약없음 | 동일사이즈(제곱) |
| 제약없음 | 수치형데이터만   |
| x        | 자동 미분        |
| x        | GPU 활용 연산    |

#### 텐서 랭크
| rank | 책에서의 언급                     |
|-----:|-----------------------------------|
|    0 | 0d tensor, scalr                  |
|    1 | 1d tensor, vector                 |
|    2 | 2d tensor, matrix, list of vector |
|    3 | 3d tensor, list of matrix         |

## [[error]]
### NotImplementedError: The operator 'aten::_linalg_solve_ex.result' is not currently implemented for the MPS device. If you want this op to be added in priority during the prototype phase of this feature, please comment on https://github.com/pytorch/pytorch/issues/77764. As a temporary fix, you can set the environment variable `PYTORCH_ENABLE_MPS_FALLBACK=1` to use the CPU as a fallback for this op. WARNING: this will be slower than running natively on MPS.
```python
NotImplementedError: The operator 'aten::_linalg_solve_ex.result' is not currently implemented for the MPS device. If you want this op to be added in priority during the prototype phase of this feature, please comment on https://github.com/pytorch/pytorch/issues/77764. As a temporary fix, you can set the environment variable `PYTORCH_ENABLE_MPS_FALLBACK=1` to use the CPU as a fallback for this op. WARNING: this will be slower than running natively on MPS.
```
+ https://forums.fast.ai/t/fastai-on-apple-m1/86059/21?page=3


### Error displaying widget
```python
cleaner = ImageClassifierCleaner(learn)
cleaner
```
+ https://forums.fast.ai/t/beginner-setup/95289/259
- python 버전 다운 필요. 3.11
- 근데 나는 3.9.6 이었는데 에러발생
- `ipywidgets` uninstall 후 커널 재시작 install 로 해결 7.x -> 8.x 로 버전업

## link
- [[ai]]
- [[jupyter]]
- [[huggingface]]
