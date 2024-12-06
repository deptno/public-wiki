# upstage-ai-lab-day-62

## deep learning
- 딥러닝 스텝
  - data
  - model
  - output
  - loss
  - optimization
- feed forward propagation
  - 데이터 입력에 따라 네트워크의 마지막까지 출력하는 과정
- back propagation
  - 오차역전파를 통해 각각의 weight 가 얼마나 loss 에 기여하는지 계산
    - 영향을 미치는 정도
  - 이 단계를 통해 weight 을 얼마나 업데이트해야할지 계산한다
- output 이 나오면 loss 함수를 통해 loss를 계산하고 오차 역전파를 통해 weight 을 업데이트 한다
  - 이 과정에서 optimizer 를 사용하여 학습속도를 가속화한다

## pytorch
- pytorch 는 딥러닝 구현을 위한 프레임워크
- 옵티마이저, 모델, loss 함수등이 구현되어있다.

## link
- [[upstage-ai-lab]]
- [[wn.private:upstage-ai-lab-project-04]]
- [[mlflow]]
- [[fastapi]]
- [[apache-airflow]]
- [[docker]]
- [[docker-compose]]
- [[upstage-ai-lab-day-57]]
- [[upstage-ai-lab-day-77]]
