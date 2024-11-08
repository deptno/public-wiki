# Image Classification 대회

## 대회
### 개요
- 3500개 정도의 이미지를 17개 클래스로 이미지 분류 후 csv 제출
- f1 macro score 가 평가 기준

### 분석
- csv 하나 제출인데 score 는 public, private 이 존재
  - public 은 아마도 제출된 csv 에서 특정 분포에 대한 점수 일 것으로 생각
    - f1 macro 기 때문에 특정 클래스에 가중을 줄 순 없다 판단
      - macro 는 클래스별 동일 가중치
    - 그렇다며 좀 더 augmentation 이 덜 적용되어 train 데이터가 효과적으로 예측할 수 있는 분포일까?
  - private 은 모델 성능을 평가해야하므로 전체 데이터 혹은 좀더 예측이 어려운 데이터를 포함한 분포일 것으로 생각

### 제공 환경
- [[vpn]]
- [[ssh]]
- AMD Ryzen Threadripper 3970X 32-Core Processor
- RTX 3090, 24Gb

## 개인적인 목표
- [X] wandb 사용
- [X] model 생성 끝까지
  - 이제 까지 프로젝트에서 모델링쪽 롤을 하지 않았으니 이번엔 모델링을 따라가 보자

## 팀
### 운영
- 각자 모델링 -> 앙상블
  - 각자의 augmentation 데이터로 모델링 -> 앙상블
  - 개인 산출물
    - model weight
    - class 예측에 대한 confusion matrix
    - f1 macro score
- 공유는 지속적으로

## EDA
- 제공된 이미지들의 크기 분포를 확인
- 훈련 데이터 셋의 이미지들의 클래스 분포를 확인
  - 14 클래스는 각 100개
  - 1  클래스가 50개 정도
  - 2  클래스가 75개 정도
- 14 개 클래스는 문서, 나머지는 자동차 번호판, 자동차 대시보드등
- 눈으로 데이터 셋 차이 확인
  - 데이터 셋 별 특징
    - train set
      - 1500 장 정도
      - 정방향
    - test dataset
      - 3000 장 정도, `train datset x 2`
      - 각종 noise 가 중첩으로 적용되어 있음
        - 회전, filp 포함
        - 가우시안 노이즈
        - 색상 변경
      - 눈으로 글자식별이 어려운 경우도 꽤 있음
      - 노이즈가 너무 심한 경우 선도 식별이 어려울 정도
  - 문서이기 때문에 각종 글자가 많이 포함됨

## 계획
- ~~ocr~~
  - 멘토의 조언에 의한
- data augmentation
  - test 에만 noise 가 포함되어 있음
    - train set 에 각종 noise 를 포함켜 양을 늘리는 augmentation 이 **필수적으로** 필요
    - noise 를 여러 조합으로 랜덤하게 늘림
  - train set 데이터 양이 더 적음
  - 데이터 부족한 class 도 있음
    - over sampling
      - 추가 증강 의미
- 팀원이 새로이 baseline 를 기반으로 데이터를 증강으로 시작
- 모델 교체 + 하이퍼파라메터 튜닝을 해가면서 진행
- f1 스코어가 높은 아이템을 모델 저장

## 멘토링
- ocr 까진 필요 없을 것 
- 모델 선택의 근거는?
  - 테스트의 영역
  - 모델군들에서 라이트한 모델을 선택해서 테스트 진행
    - 각 모델군은 경향성을 보이기 때문
    - 대충 파라메터 모델 튜닝후 모델 선택
- loss 는 데이터에 imbalance가 있으니
  - focal loss
  - asymmetric loss
  - weighted cross entropy

## 진행
- augmentation
  - 훈련 이미지 1,500 개 -> 팀원 마다 80,000 ~ 180,000 개 수준으로 증강
- model
  - 후보선정
    - 멘토링때 언급된 모델
    - sota
    - 팀원과 겹치지 않는 것도 앙상블을 위해 의미있을 것
  - 선택
    - *efficientnet-b5* 를 선택
      - 이유 
        - 팀원과 겹치지 않는 모델
        - 이미지가 눈으로 식별이 어려워서 resnet34 등이 학습한 `224`x`224` 해상도로는 식별이 안되 분류가 어려울 것으로 예상
        - 문서의 복잡도를 감안하여 test dataset 에 가까운 이미지 크기로 학습된 모델을 사용하고자 함
          - 24gb 의 rtx3090 으로는 `b7`, `b6` 에 대한 학습 진행이 불가하여 `b5` 를 차선으로 선택
          - `batchsize=16` 에서 훈련 가능한 것 확인
- modeling
  - 초반에는 팀원이 baseline 코드로 제공한 *convnext* 모델로 데이터만 증강하여 진행
  - *efficientnet-b5* 선택 이후 성능을 더 올리기 위해 추가 데이터를 증강
  - 이 과정에서 데이터도 versioning 이 필요하다는 것을 직감
    - 이미 팀원은 그렇게 하시는 분이 존재
    - 데이터 증강후 눈으로 샘플 확인, 분포 그래프 화를 위해 [[jupyter-notebook]] 로 진행
      - jupyter-notebook 특성상 버전관리([[git]])가 어려워 파일명에 버저닝을 붙여 복사 진행
      - 생성되는 데이터도 버전 디렉토리를 만들어서 생성
  - 모델링 진행
    - 1 epoch 기준 40분 정도 소요
- 사고
  - 서버가 갑자기 접속이 되지 않고 메시지 웹을 보니
    ```
    The server was shut down due to excessive use of disk or memory. Due to the nature of the system, data recovery is not possible, so please create a new server and use it.
    ```
  - 문의 결과 데이터 복구 불가, 상시 백업하라는 피드백
  - 마지막 날이라 복구가 불가능해서 모델 뽑지 못함
- 이후
  - 난 서버가 터져서 작업본이 날아갔기 때문에
    - 팀원들이 제출한 csv 파일에서 서로 틀리게 예측한 것들을 diff
    - 이를 시각화하여 어떤 것을 예측못하나 확인
  - 마지막날이니만큼 팀원들이 각자의 데이터에서 훈련한 각자의 모델 가중치를 가지고 앙상블
    - 앙상블 할때마다 연금술 마냥 public f1 score가 상승
    - 팀원들이 생성한 모델, 전부다 앙상블에 참여한 것은 아님
      - *convnext_tiny*
      - *densenet121*
      - *efficientnet_b0*
      - *mobilenet_v2*
      - *mobilenet_v3*
      - *resnet18*
      - *resnet50*
      - *shufflenet*
      - *squeezenet*

## 최종 결과
- 1등
  - leader board
    - public 2등
    - private 1등

## 회고
- ssh 개발 환경
  - [[pycharm]] 은 유료 버전인데도 [[ssh]] 개발환경에서는 [[jupyter-notebook]] 파일에서 [[markdown]] cell 을 표시 못함
    - [[vscode]] 로 진행
  - 기본적인 것은 서버에 몇개 바로 설치하고 가는게 빠르다
    - [[tmux]] 
    - [[vim]]
    - [[lazygit]]
  - 데이터 셋도 서버에 있다보니 사실 눈으로 확인만 하면되는데 [[jupyter-notebook]] 을 통해 불편하게 확인해서 시간이 소모
    - local 에서 서버 디렉토리를 [[mount]] 해버렸어도 되었을 것 같고
    - [[jupyter-notebook]] 에서 코드로 시각화 하느니
      - target class 로 분류된 이미지들을 각각의 target class 디렉토리에 나눠서 이미지 뷰어로 확인하는게 나은것 같다
        - [[scp]] 로 로컬 전송후 확인
- eda
  - 내가 어떤 데이터를 분류하려고하는지, 어떤 노이즈가 있는지를 시각적으로 확인
  - class 분포를 가지고 loss 함수에 대한 선택 인사이트 제공
    - 따라가기 급급해서 나는 그냥 cross entropy 씀
    - 다음에는 제대로된 loss 도 적용해보자
  - 데이터 해상도를 시각화하여 어떤 모델(규모)을 선택할지에 대한 인사이트
    - [ ] 이건 이론적으로 의미있는지 검증되지 않은 내 생각
      - [[chatgpt]] 는 긍정적으로 답변
- 서버 터짐 사고
  - 서버에 [[git]] 을 설치해서 버전관리를 하고 있었는데 서버가 깨질것을 상상하지 못해 push를 하지 않아 결과를 만들지 못함
  - remote 에 주기적으로 [[git-push]] 하자
- 일정 관리
  - 대회 기간전 학습 마지막주에 서버가 열리는데 이때 학습이 아닌 대회에 집중을 해야한다
    - gpu 사용과 submission 이 시간에 비례하기 때문
    - 어차피 수업이 논문 설명이 갑자기 시작되는 구조기 때문에 사전지식이 없다면 시간낭비일 가능성이 높다
      - 먼저 손으로 해봐야 이론에 대한 니즈가 생길 것
    - 프로토타입은 먼저 만들어서 구동하고 모델 돌리면서 수업을 듣던가 하자
- 코드 익숙도
  - `albumentations` 라는 라이브러리를 썼는데
    - `mix-up` noise 생성을 지원하지 않아 해당 noise 는 코드로 구현해야 하는 부분에서 시간이 다소 소요되었다
    - 이 부분은 코드를 적극적으로 쓰면서 늘어갈 것 기대
- 모델링
  - 모델 선택에 대한 근거가 부족했는데 멘토링때 질문을 통해 다소 해소되었다
  - 모델보다는 데이터에 대한 중요성이 우선
  - 지금 까지의 경험으로는 모델링은 틀이 있어서 이에 대해 익숙해지고 프로토타입 코드를 빠르게 뽑도록 익숙해져야 유리
- 툴
  - 강의나 특강에서도 계속적으로 언급되는 wandb 를 이번에 맛보기로 사용
    - wandb 는 파라메터 튜닝을 지원
      - 그럼 optuna 는?
  - 이전 강의 때 배운 [[mlflow]] 가 이쁘진 않지만 이런 실험에서 많은 고민을 덜어줄 수 있을 것 같다고 생각
  - wandb vs [[mlflow]]
    - [[mlflow]] 는
      - 좀 찾아보니 linux foundation 재단에서 운영
      - [[github]] star 가 가장 많음
      - ui 이가 상대적으로 구려서 선택에 고민이 있는 사람들이 많은 것 같음
      - 추가적으로 model registry 를 지원하는 점이 wandb 와 구분되는 특징으로 선택의 기준이 되기도 했었던듯
        - 그러나 지금은 wandb 도 artifact 저장을 지원
          - cloud 5gb 무료
  - 필요한 기능은?
    - 파라메터 튜닝에 대한 시각화 기록
    - 모델 가중치 저장
  - 필요한 기능은 어차피 지원하는 거 같고 하나를 정해서 쓰면 될텐데
    - 가중치는 여러개 저장하기에 5gb는 힘들 수 있으니 [[mlflow]] 를 환경으로 가져가는게 좋겠음
- 결과에서
  - 이번에는 computer-vision 석사 전공자 분이 계서 리드를 받으면서 캐리해주심
  - 딥러닝이나 머신러닝 모두 데이터처리, loss, 하이퍼파라메터 튜닝, 앙상블등 큰흐름이 같음
    - 좀 더 능력자를 빨리 만나는게 이 프로세스를 빨리 경험하고 이해하는게 상황을 많이 바꿧을 것이란 생각하니 안타까움이 반

## link
- [[upstage-ai-lab]]
- [[fastapi]]
- [[docker]]
- [[docker-compose]]
- [[upstage-ai-lab-project-03]]
- [[upstage-ai-lab-project-05]]
- [[wn.private:upstage-ai-lab-project-04]]
