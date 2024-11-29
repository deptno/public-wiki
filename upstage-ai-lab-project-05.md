# NLP Summary 대회

## 대회
### 개요
- DialogSum 번역 데이터를 통해 요약문을 생성하여 정답지와의 ROUGE-F1 스코어를 높인다
- 요약문은 원문의 20% 내외

### 분석
- 평가는 dialogue 하나당 각기 다른 포커스를 가진 3개의 summary 가 있고 이를 통해 점수를 낸다고함
  - 설명이 두루뭉실하여 완전히 파악 못함
- 데이터셋은 dialogue, summary 외에도 핵심 단어일 것으로 추측되는 topic 이 주어짐
- wandb 경험을 위해 제공된 `baseline` 의 코드에서 `wandb` 설정을 가지고 있음

### 제공 환경
- [[vpn]]
- [[ssh]]
- AMD Ryzen Threadripper 3970X 32-Core Processor
- RTX 3090, 24Gb

## 개인적인 목표
- [X] 대회전에 강의 다 듣기
- [X] 시스템
  - [X] mlflow 적용
  - [X] server 에 job queue 적용
- [ ] 대회 잘 해보기

## 팀
### 운영
- 각자 해보고 증강 데이터는 공유
- 2주에 걸친 대회기간 중 1주는 수업이 이 겹치므로 이 기간은 각자해보고 마지막 한주는 공유하면서 진행

## EDA
|      | dataset | dialogue | summary |                            |
|------|---------|----------|---------|----------------------------|
| 왜도 |         | right    | right   |                            |
| 크기 | train   | 12457    | 12457   | `topic` 포함               |
|      | valid   | 499      | 499     | `topic` 포함               |
|      | test    | 499      | 499     | public(250) / private(249) |
| 어체 |         | 구어체   | 평문    |                            |


## 계획
- GPU 사용 시간을 최적화 할 수 있는 시스템을 가지고 시작
- 대회의 점수 방식과 유사한 방식의 평가 방식을 로컬에 적용후 시작
- [[mlflow]] 활용을 잘 해보기

## 멘토링 :mentoring:
- 여러가지 코멘트가 많았지만 중요한 점을 요약해 본다면 2가지
- 모델 추천
  - 한국어 모델 써봐라, llm, back translation 을 위한 번역 모델, 요약 모델
    - lg exaone
    - nllb en ko
    - m2m
    - pko-t5 등
- 내가 궁금한 점에 대한 답변
  - 강화학습은 훈련데이터로 한번 더하는거냐
    - 그렇다 근데 하지마라
  - 생성해야하는 길이가 다 다른데
    - data collator 를 사용하면 유사한 길이 끼리 모아준다
- 그리고
  - 증강된 데이터의 품질이 좋아야한다
  - 요약 대회라기보단 요약 정답지에 맞추는게 목표
  - 하이퍼파라메터 튜닝은 마지막에 하는 것

## 진행
### 팀원
- 데이터 증강, 특히 back translation에 집중
  - 시도
    - Random Masking Insertion
    - 여러가지 adverb 교체
    - back translation
  - back translation 외에는 효과가 별로
    - 영어 to 한글 외에도 어순이 다른 언어를 적용
- kogamza 등 다른 kobart 시도
- pko-t5 계열 시도

### 나
- [[mlflow]], [[wandb]] 적용
  - [[mlflow]] 적용하면서 [[kubernetes]] 에 오리는데 여러가지 try-and-error 가 있었음
  - [[mlflow]] 는 하나하나 해줘야하는 설정이 많음, 마지막까지 시간 관계상 처리하지 못한건 시스템 로그 보기
- [[vscode]], [[vim]] binding을 사용하는데 함수를 못 따라가는 것이 너무 치명적이고 왔다가 갔다하기 어려워서 스크립트 코드로 진행
  - 이 부분은 아래 [[#retrospective]]
- local ROUGE-F1 스코어가 public score 와 괴리가 심함
  - rouge score 계산시에 kkma 형태소 분석기를 적용하여 점수를 근접하게 맞춤
    - konlpy 에 다섯개의 형태소 분석기 중 가장 점수가 높게나온 kkma 를 사용
    - 2개는 에러가 나서 사용하지 못했는데 그중 mecab 이 표준일 거라함, [[#mentoring]]
- 스페셜 토큰은 전체 16개로 검색되었지만 적용시에 점수가 오히려 떨어졌음
- 팀원과 겹치지 않는 증강 시도
  - solar credit($10)를 사용하여
    - solar LLM 을 이용
      - pro 는 영어 전용 모델인 것 같고 mini 로 진행
      - 말을 알아듣는거같으나 뒤에 쓸때 없는 문장들이 끌려 오는 문제
      - 프롬프트 엔지니어링을 고도화 할 것이냐 -> 그냥 번역 전용 api를 사용하기로함
    - back translation 진행
      - translation 전용 api로 진행
        - 방심한 나머지 제공된 $10 로는 데이터 데이터셋 bt가 불가능한 걸 늦게 인지, ~~왜준겨~~..
        - 그나마 확인된 결과도 few shot 주입이 안되서 special token 도 같이 번역되어 버림
          - 전, 후처리를 하거나 여러가지 복잡한 처리가 필요
          - 아 이러면 그냥 프롬프트 엔지니어링을 했어야
  - 학습시에 summary 길이를 스페셜 토큰으로 만들어 원문에 삽입
    - 멘토링시 답변이 회의적이라 적용하지 않음
- 멘토링 이후 추천받은 적용
  - pko-t5 적용
    - 막상 적용을 하려고보니 그냥 모델만 갈아끼면 되는게 아니라 데이터 입력형식이 모델마다 존쟇는 것으로 보임
    - 이를 돌릴때는 이미 팀원이 조금 상위권의 점수를 확보
    - 딱히 같은 모델+데이터 돌리는 것이 없을 껏 같아 시스템 고도화 몰입

## 최종 결과
- 2등
  - leader board
    - public 2등(0.5855	0.4070	0.5167	50.3062)
    - private 2등(	0.5715	0.3799	0.4826	47.8024)

## 회고 :retrospective:
- llm 분야에서는 상용 llm 파인튜닝이 최고의 결과를 도출할 것 같다.
  - 목표를 한정해서 하나의 타스크를 수행한다면 못할 것은 없어보인다
  - 다만 이번대회가 이런 경험을 가져가기에는 어렵게 설계된 느낌이 있다.
- 베이스라인을 넘기는것 자체가 어려웠고, 처음에 시드 고정이 안되서 조금 헤멘 느낌이 있다.
  - 반드시 시드고정 먼저 진행
- data 기반이 어려운 문제라(원문 -> 영어 번역으로 대회 진행)
  - 결국 모델 싸움으로 가는 느낌이고 그런경우 상용모델을 이기기 어려워 동기부여가 떨어졌다
- 아래 내용은 이전 회고로 부터의 피드백 반영
  - 이전 프로 젝트 회고
    + [[upstage-ai-lab-project-04#retrospective]]
  - ssh 개발 환경
    - 이번엔 스크립트로 워커 큐를 만들어서 py 스크립트를 큐에쌓아서 지속적으로 실행할 수 있도록했다
      - 이를 위해 처음에는 [[jupyter#nbconvert]] 를 활용한 스크립트 변환 활용
      - 이후에는 eda 를 제외한 영역은 스크립트로 작성해사 사용했다.
    - [[chatgpt]] 가 코드에디어와 협업을 지원하기 시작
      - 시작 시점에는 [[vscode]] 만 지원 -> 대회 도중에 [[pycharm]] 지원이 업데이트 됨
      - 로컬만 프로젝트만 지원해서 로컬 코드 작성후 -> [[scp]] 를 활용한 파일 복사를 사용
    - 저번에 서버 터졌으므로 [[git]] 에 차곡차곡 저장
  - eda
    - 데이터를 시각화를 먼저하고 가설 설계
      - 길이 토큰
  - 일정 관리
    - 대회 시작전 강의 먼저 모두 수강하여 시간 확보
  - 코드 익숙도
    - 코드를 먼저 분석하고 들어갔으나 여전히 jupyter 로 큰파일을 진행하기에는 ide도움이 너무 떨어지는 문제
    - 다음 프로젝트 에서는
      - [ ] [[mlflow]] 나 저장, 평가등 일부 루틴화된 영역을 처음에 분리하고 수정포인트 업데이트 하는 방식이 좋겠다
  - 모델링
    - 역시 모델링 선택에 대한 근거는 여전히 부족
  - 툴
    - wandb 사용을 하며 서로 비교할 수 있었다
    - [[mlflow]]
      - 많은 해결을 하였고 다음엔 system log만 추가적으로 저장하면 될 것같다
      - wandb 와 달리 sweep 이 없어서 이부분은 [[optuna]] 를 적용해 보았다
        - 파라메터 튜닝 별로 할게 없어서 적용하는 경험에 의의
  - 결과
    - 이번엔 저번과 달리 데이터 증강이나, 앙상블에 의한 변화가 적은 프로젝트
    - 모델의 중요성을 크다는 것을 느낌
    - 무엇보다 반복실험에 있어서는 효율적인 시스템과 툴 사용을 통한 기록의 시각화가 많은 도움이 되었음
    - [ ] 다음에는 반복적인 실험에 대한 계획을 잘짜서 만들어진 시스템을 활용해 보아야겠다

## link
- [[upstage-ai-lab]]
- [[fastapi]]
- [[docker]]
- [[docker-compose]]
- [[upstage-ai-lab-project-04]]
- [[upstage-ai-lab-project-06]]
- [[wn.private:upstage-ai-lab-project-05]]