# practical-deep-learning
+ https://course.fast.ai
+ https://github.com/deptno/study-ai

## 01. getting started
- 이미지 분석기 따라 생성해보기
- imagenet 을 학습한 기본 모델, resnet18 기반 개/고양이이를 추가학습(파인튜닝)

## 02. deployment
- huggingface 에 gradio 등을 통한 app + api endpoint 배포
- Image Classifier Cleaner, 학습시 잘못된 데이터 제거
- Data augmentation -> 데이터 증강, 이미지를 랜덤 사이즈 크롭, 혹은 변형을 통해 학습을 증대시킨다
- leaner 를 pickle 파일로 export -> import 사용

## 03. nautral net foundations
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

## 04. natural language (NLP)

## link
- [[ai]]
- [[jupyter]]
