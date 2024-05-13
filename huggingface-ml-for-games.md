# huggingface ml for games

## UNIT 0. WELCOME TO THE COURSCE
### Welcome to the course 🤗
- AI 모델로 하는 혁신
  - How we make games
    - 텍스쳐 생성
    - AI 보이스 사용
  - How we create gameplay
    - LLM 백엔드의 NPC
- 배우게될 것
  - 챗 모델을 사용한 NPC
  - 모델의 로컬 및 클라우드 API 사용
  - AI 툴들, TTS, 이미지 생성
  - 게임 데모 생성
- 요구사항
  - 디스코드 조인
  - 유니티 개발
    - https://learn.unity.com/course/create-with-code (36시간 30분)
- 이 코스에서 다루진 않지만 딥하게 가고자 한다면 추가 학습이 열려있다
  - NLP
  - Audio
  - Gradio
- 코스의 목표
  - 자신만의 게임 데모
  - AI 를 활용한 게임 제작
  - AI 를 활용한 게임 플레이(NPC등)

### The Goal, Build Your own Game Demo
- 자격증 조건
  - 5명 이내의 팀
  - WebGL 혹은 Windows 에서 구동
  - English
  - 1개 이상의 오픈소스 툴 활용
- Unity 
  + https://learn.unity.com/course/create-with-code

### Syllabus
- 강의구성
  - 이론
    - ML 을 딥하게 다루는 것이 아님을 알 것
    - 허깅 페이스의 추론 api 를 사용할 것으로 예상됨
  - 핸즈온
- 각 유닛은 1주(주당 3-4시간)으로 진행되는 페이스로 구성
- 데모 구현을 위해서는 2-4주의 시간을 사용할 것을 권장

### The Team
- 강사 소개

### Onboarding
- 할일
  - 허깅페이스 계정 생성
  - 디스코드 서버 조인

#### 디스코드 채널

|                                             |                            |
|---------------------------------------------|----------------------------|
| ml-4-games-course-announcements             | 최신정보                   |
| ml-4-games-course-announcements-study-group | 질문 및 의견 교환          |
| ml-4-games-dev                              | ML 게임 개발에 대한 교환   |
| ml-4-games-searching-for-team               | 데모 구현을 위한 팀원 찾기 |
| ml-4-games-i-made-this                      | 내 모델 공유               |

### Discord 101
- 주저하지말고 자주 참여해라

### Getting the most of the course
- 따라서 구현할 것
- 수정을 주저하지 말것
- 팀으로 할것을 권고

### Conclusion
- 배우게될 것
  - Unity Sentis 와 로컬에서 AI 모델 사용 및 첫번째 지능형 AI NPC
  - 데모 게임에 대한 게임 디자인 문서 작성

## UNIT 1. CREATE A SMART ROBOT NPC USING HUGGING FACE 🤗 AND UNITY SENTIS
### Introduction
- 배우게 될 것
  - Sentence Similarity
  - AI 모델의 로컬, 클라우드 실행 차이
  - 허깅페이스
  - Unity Sentis, Sharp Transformers 와 함께 AI 모델을 로컬에서 실행하는 방법
- 사용하는 것
  - 유니티 게임 엔진 2022.3+ 
  - Jammo Robot 어셋
  - Unity Sentis library - AI 모델을 게임안에서 실행하기 위해 필요
  - Hugging Face Sharp Transformers -  유니티 게임 안에서 허깅페이스 트랜스포머를 실행할 수 있게 하는 유니티 플러그인
- 예제 게임 설명
  - 채팅으로 NPC 에게 요청하고 NPC 가 이를 실행
  - 가져다 달라거나 춤춰달라거나 요청 후 이를 NPC 가 실행
  - 건물안에서 가드를 피해서 숨고 목적 아이템을 훔쳐오라는 지시 후 NPC 가 이를 수행하는 게임

### The Power of Sentence Similarity 🤖
- 게임만들기 전에 문장 유사도에 대한 정의와 동작을 이해해야한다

#### How does this game work?
- 선택지를 클릭하는 대신 챗을 통해 더 많은 자유도를 제공한다
- 로봇은 액션 리스트를 가지고 있고 유저의 채팅문장과 유사도가 높은 액션을 선택해서 사용한다

#### What is Sentence Similarity?
- 주어진 문장(input) 과 타겟 문장들간의 유사도를 계산하는 것
- 입력 문장 -> embed -> 임베딩(벡터)
  - 이를 가지고 cosine 유사도를 계산
  - 딥하게 들어가고 싶다면 링크 참조
    + https://huggingface.co/tasks/sentence-similarity

#### The Complete pipeline
- 유저 인풋과 액션 리스트는 모두 string 으로 되어있다.
- 유사도 처리를 위해서는 임베딩이 필요하며 이는 *Sharp Transformers* 에서 처리한다
- *Sharp Transformers* 를 통해 토큰화된 인풋은 *Unity Sentis* 모델의 입력으로 들어간다
- *Unity Sentis* 모델의 결과로 유사도 점수가 나온다

1. 유저가 명령어를 입력한다: "Can you bring me the red cube?"
2. 로봇은 액션 목록을 가지고 있다
  - Hello
  - Happy
  - Bring red box
  - Move to blue pillar
3. 가장 유사한 액션을 찾기 위해 인풋을 임베딩한다
4. *Sharp Transformers* 에 의해 입력이 토큰화된다
5. 토큰화된 입력을 모델에 넣어 인풋에 대한 정보를 갖는 임베딩을 출력한다(by *Unity Sentis*)
6. 임베딩 되었으므로 유사도 검색이 가능해진다
7. 유사도 검색(비교) 에서 가장 높은 스코어를 가진 액션을 선택한다  
  - 해당 스코어가 `0.2` 이상인 경우 액션을 실행을 요구한다
  - 해당 스코어가 `0.2` 이하인 경우 "몰루~" 처리한다

#### Why `0.2` for the Similarity score threshold?
- 여러 테스트 결과를 통해 찾아진 값이다
- 스코어는 액션 리스트 전체를 합치면 1이 된다, 때문에 액션 리스트가 늘어나는 경우 threshold 값은 낮아져야한다

## DEFINING MY DEMO PART 1. FINDING THE IDEA AND WRITING THE GDD
### How to run an AI model: local vs remote
- 이 게임에서 유사도 검색을 사용하기 위해 `all-MiniLM-L6-v2` 모델을 사용한다
  + https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2
- 이 모델은 BERT 트랜스포머 모델로 이미 학습을 마쳤으므로 사용하면된다

### Running the model remotely
- 서버 api 를 호출하는 방식
- 허깅페이스에서 *Inference API* 를 제공
  - 프로토타입및 테스트용은 무료
  - 이를 사용하기 위한 유니티 플러그인이 존재
    + https://huggingface.co/blog/unity-api
- 장점
  - 클라이언트 측에 리소스 필요하지 않음
  - 서버쪽에서 로깅할 수 있음으로 유저 입력에 대한 정보등을 개선에 쓸 수 있음
- 단점
  - 인터넷 연결 및 서버 상태에 따른 종속이 생김
  - 잠재적 비용 비용 폭탄 가능성
- 주로 클라이언트에서 실행할 수 없는 큰 모델을 실행하는 경우 API 제공이 필요하다

### Running the model locally
- 클라이언트에서 사용하기 위해 2개의 라이브러리를 사용한다
  - *Unity Sentis* - AI 모델을 게임 안에서 실행할 수 있도록 한다
  - *The Hugging Face Sharp Transformers library* - 유니티 게임에서 트랜스포머 모델을 사용할 수 있도록 한다
- 장점
  - 돈 안나감
  - 인터넷 연결 종속 없음
- 단점
  - 클라이언트 스펙에 영향을 받는다(RAM/VRAM)
  - 유저가 어떤식으로 모델을 활용하는 정보를 쉽게 얻지 못한다

- 유사도 검색은 가벼운 모델이라 **로컬에서 사용하기로한다**

## BONUS 1. CLASSICAL AI IN VIDEO GAMES

## UNIT 2. AI TOOLS FOR GAME DEVELOPERS 🎨

## link
- [[huggingface]]
