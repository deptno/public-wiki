# upstage-ai-lab-day-43
- machine learning basic
- machine learning advanced
- 캐글, 대회 관련 특강
- **House Price Prediction** 경진대회

## 2주간의 수업
- 기본이 없는 경우 수업을 들으면서 무엇에 쓰이는지를 상상하기 어렵기 때문에 효율이 떨어진다
- 때문에 졸리게 되는데 진도를 뺀다는 생각으로 들어봤자 졸면서 해봤자 기억에 남지 않는다.
- *learn by doing* 이라고 오히려 실습, 혹은 실제 프로젝트를 들어가서야 *아! 그거* 하는 순간이 찾아오고 찾게된다.

## 대회: House Price Prediction
> 대회의 EDA 시도나 결과는 모두 유사할 수 밖에 없어, 개인적인 경험을 위주로 기록한다

### 대회
- 대회기간은 2주다
- 2주는 한주는 수업을 들어야해서 입문자를 기준으로는 기간이 1주다
- upstage 에서 주관하므로 https://stages.ai 를 통해 진행된다
  - 한번은 제출 결과가 영원히 `pending` 에 빠지는 문제가있었으나 재업로드로 해결할 수 있었으므로 크리티컬한 문제는 아니었다.
- 대회는 일별로 제출 한도가 있다. 때문에 뒤늦게 하려고한들 팀원들이 몰릴테고 나에게 돌아오는 기회는 작아지게된다
  - 때문에 피쳐 엔지니어링에 들어가기전에 한사이클을 돌릴 수 있도록 모델까지의 파이프라인을 생성해야한다
  - 그럼 서버는 파이프라인으로 매번 돌아가고 난 그사이에 피쳐 엔지니어링을 통해 다음 타스크를 머신에 부여할 수 있다.
- 주최측에서 제공하는 서버는 **nvidia RTX3090** 을 포함하고있었다.
  - `nvidia-smi` 는 설치되어 있었으나 `nvcc` 명령어가 없어 추가 설정을 해야 gpu 를 활용할 수 있었다.
    - 이런 설정도 주최측의 의도인건가?

### 협업
- 팀바팀이지만 우리 팀에 능력자는 없었다. 학부때 경험자는 있었다.
- 특별한 리더쉽이 없는 상태에서 스크럼은 유야무야되어 5일동안 2,3번 정도 초기에 진행되었다.
  - 소통이 별로 없다보니 서로 공유하거나 이런 것이 제한되었다.
- [[git]] 이 활용되기 어려워서 파일들은 주로 [[slack]] 을 통해 전달되었다
  - 팀원들의 경험
  - [[jupyter-notebook]] 이 실행 결과를 포함하는 포맷이어서 생기는 문제
- 개발 환경
  - os 부터 시작해서 모두 각자 다른환경을 가지고 있고 이걸 통합하지 못함으로 인해서 로컬 실행이 안되는 분도 있었다.

### 코드
- jupyter-notebook 을 통해 모든 것을 진행하게 되면 스파게티 코드와 같은 경험을 하게된다
  - 이 것이 이분야의 일반적인 경험인지 모르겟으나 나로썬 집중하기 어렵고 비효율이 집중을 흐리게했다.
    - 너무 길다, 마킹을 위해서는 중간 중간을 `markdown` 으로 헤더를 남겨서 IDE 기능을 통해서 이동해야한다
    - 수정을 어디했는지를 잘 모르니 어디서 부터 다시 시작해야할지 헷갈린다
    - `stateful` 한 방식의 코드가 셀별로 사용되다가 어디까지 실행되었는지 잃은경우 다시 시작한다
  - 이러한 것들은 이런 프로젝트의 프로세스가 체화되면 나아질 수 있을 것 같다
  - 개발자가 가지는 모듈화라는 관점에서는 좀 어려운 면이 있었다
  - `vscode` 는 좀 나은 것 같긴한데 `pycharm` + `vim` 의 조합은 최악이었다. 버그투성이고 커서가 튀어서 집중하기가 어려웠다.

### 경험
- 각개 격파를 하는 중에 [[chatgpt]] 와의 심도깊은 대화를 통해 많은 것을 깨달았으나 이미 늦었다.
- 데이터 분석도 실습없이 눈으로 보고 온 것들이 지금와서 발목을 잡는다
  - 실습하고왔어도 어차피 같은 상황이었을 것 같긴한다
  - 어차피 익숙해지는데는 시간이 걸린다
  - 프로젝트가 시작되기 전에 눈으로 보는 수준으론 어렵고 손으로 정리하면서 이해의 수준을 끌어 올려두면 도움이 될 것
- 프로젝트를 진행하면서 필요한 task는 순서가 분명히 존재하니 이를 체화해야한다.
  - 효율성 때문, 결측치를 먼저 보정한뒤에 그래프를 본다 등
- jupyter-notebook 
  - [ ] jupyter-notebook 의 결과에 대한 커밋을 strip 하는 방식으로 진행해야할지 확인이 필요하다
    - [ ] diff 툴도 찾아본다
  - 한 파일이 너무 길어지면 파일 관리가 어려워지는 것은 매 한가지인 것 같다
    - 그러나 캐글등 모들 플랫폼에서 원파일로 정리가 되어있다.
      - [ ] 정리만 한 파일로 한걸가?
  - 하이퍼파라메터 튜닝등 time-consuming 한 작업을 하기위해 `py` 파일이 편리할 것 
    - [[tmux]] 와 같은 가상 세션으로 집전기세는 보존
  - 아무래도 eda 나 피쳐 셀렉션등의 결과를 스탭단위로 저장하고 다음 파이프라인(파일)로 가는 형태로 설계하는게 좋을것이라 생각된다

### 정리
#### 작업 순서를 지켜야한다
- 특히 처음에는 어떤 것을 할지 떠오르지 않고 여러 검색결과나 다른 사람들의 작업을 물을 보다보면 점점 더 헷갈릴 수 있으니 순서를 체화해서 진행한다

##### 데이터 전처리
> 데이터 확인후, 이상 데이터부터 처리후에 엔코딩 -> 변수생성, 선택으로 간다
- [ ] [[@todo]] 엔코딩이 왜 먼저인지는 추후 찾아보자
1. EDA
2. 결측치 제거
3. 이상치 제거
4. 카테고리 변수 엔코딩
5. 연속형 변수 엔코딩
6. 파생 변수 생성
7. 변수 선택

##### 모델 선택
> 이 부분 부터는 컴퓨터와 페어가 가능하다, 동시에 일을 처리하는 것이 중요
- 적합한 모델을 생성
- 하이퍼 파라메터 생성

##### 학습 평가
- 데이터 셋 분할
- 모델 성능 측정

#### 발표 피드백
- 김현우 강사
- test set 조건 부터 확인
  - 기간등
    - 타겟 기간이 명확하기 때문에, 타임시리즈 스플릿 사용이 명확하고 점수가 낮다면 다른 부분을 찾아봐야한다
- scale 은 트리모델에서는 관련이 없음
- catboost 가 카테고리 관련성이 큰 경우 힘을 발휘할 수 있다.
- 결측치는 트리계열에서는 드랍하지않아도 알아서 우선순위가 낮아진다는듯?
- 구별 -> 평균값을 기준으로 라벨링 한 팀들이 좀 있음
- feature importance, lightgbm 기준
  - [ ] option 확인할 것
    - 개인
    - split
  - 변수 중요도는 어떤 로직(옵션, 알고리즘) 사용에 따라 달라지니 함께 분석해야한다
- 개발환경
  - 로컬 실험 환경을 맞춰줘야한다
    - valid set 결정을 함께해서 파일로 공유하며 좋다
  - 가상환경 맞추기
- 하이퍼파라메터 튜닝
  - `n_estimator` 는 10만등 크게 고정하고 early stopping 으로 조절하는게 일반적
- 앙상블
  - 동일한 부스팅계열이라던가하는경우 성능이 떨어지긴한다
- 시계열데이터인지 아니면 좌표게임인지 판단이 다르다, 지형영향을 더 많이 받는다고도 볼 수 있기 때문

##### 리뷰
- 목표
  - tabluar 데이터를 기반으로 머신러닝 프로세스에 대한 경험
    - 결측, 이상데이터 처리
    - 모델 선택
    - 데이터셋 분할, 이게 1번
      - 이게 아마도 제일 중요하다
      - valid, test 셋 점수 차이가 많이 나는 경우 상위랭커는 해당 대회 참여를 잘 안함
    - 하이퍼파라메터 튜닝
      - 모델별로 중요변수는 정해져있다
      - lightgbm 인 경우
        - learning rate
        - max_depth, num_leaves, total_leaves
          - over, under fitting 방지를 위한
  - 이상거래
    - 분양가와 실거래가가 혼재되어있는 경우
    - 실거래가 찍혔으나 실제 거래는 되지 않은 경우
- 대회의 어려운점
  - 부동산은 불편, 하지만 외부가치가 변한다
    - 지하철역이 어느시점에 생김
  - 표현되지 않는 데이터
    - 오션뷰
    - 팬트하우스
- 강사의 직접 후기
  - 결측치, 카테고리 빈값 채우기
  - stage1 timeseries 검증, 크기랑 *월수(3달)을 맞춰준다*
    - stage2 풀데이터로 다시 훈련 
      - 최근 3개월이 가장중요한데 해당 데이터가 valid 셋으로 빠진것을 다시 충전하기위해
      - stage1 의 하이퍼파라메터를 그대로 사용
    - 결측치 부터 다시 채우기 사이클
  - 문제의식
    - validation, leader board rmse 차이가 크다
    - 예측 실패 이유
      - 학습에서 못 본 데이터
      - 학습에 있으나 거래된지 오래된 경우
          - 최근 거래를 기반으로 평균값 조치?
  - 결측치
    - 좌표가 결측치인 경우 오차가 심한지 비교
  - public / private
    - 15000 / 11000
  - 시간없어서 실패한 시도
    - 네이버 부동산, 강변, 층 정보등 크롤링, 시간 문제로 실패
    - 서로다른 모델들, vision, text + nn
      - image -> image encoder -> extract image features -> feature imbedding -> feature
        - 너무 크면 PCA 진행
      - 자연어 -> 텍스트 임베딩 -> 피쳐화
      - nn, lightgbm 은 차이가 있으나 참고
    - 고가, 저가, 지역별 분할 후 앙상블
  - Q&A
    - 없어진 아파트 같은 경우면 제거한다
    - 근처 직전거래가 가격을 많이 변영하기 때문에 잘 활용하며 좋을 것 같다 -> 오버피팅이 심해서 주의해야했다
    - 슈로 레이블링이 왜 금지인가?
      - 미래 데이터를 활용한 예측의 경우 데이터 리키지 문제로 인해서 금지되는 경우되는 경우가 있다
    - 직전변화율을 추적하는게 의미있겠나?
      - 가격자체보다 더 의미가 있을 것 같다
    - 리더보드 점수가 안 오를 것 같을 때
      - 금메달은 둘다해야한다는 듯, 팀이면 나눠서 진행해도 좋다, 멘탈 관리해라
        - [ ] 디테일 조정하냐
        - [ ] 파괴적인 방법을 시도하냐

#### 결론
- 데이터 전처리부터 학습평가까지 진행하면서 좋고 나쁨을 테스트할 수 있도록 파이프라인을 구성해서 서버를 쉬지 않고 돌리도록 한다
- 데이터 분석을 위해서 jupyter 를 쓰되 중간중간 결과를 출력해서 여러 파일로 나눠서하는 것이 유연성이 높을 것 같다
  - [[pycharm]] 기준으로 에러가 너무 심해서 중간중간에 의도치 않은 영역이 수정될 수 있고 변경 추적이 어렵기 때문

## link
- [[upstage-ai-lab]]
- [[upstage-ai-lab-day-28]]
- [[upstage-ai-lab-day-48]]
