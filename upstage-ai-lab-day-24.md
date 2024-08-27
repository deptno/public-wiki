# upstage-ai-lab-day-24

## 녹화학습
- rag
- rag 강의 내용은 집중하기가 좀 어려웠다.
- 코드로 보면 이해하기 나쁘지 않았다.
- 이미 관련 도서를 몇권 읽었었고 실제로 이런 형태의 구현이 실망스럽긴했으나 유용한 서비스 구현을 가능할 것으로 보이고 이를 경험한 것은 좋았다.
  - 실망스러운 부분
    - 강의 내용이 아니라 rag 필요성 자체
    - llm 이 특정 시점을 기준으로 학습되는 약점을 커버하기 위해 루틴한 부분이 구현되어야하는 부분

## 프로젝트: rag
- 첫번째 팀 프로젝트
- 4일
- 프로젝트 자체는 개발 프로젝트이기 때문에 배운 내용과 별개로 [[langchain]] 문서 기반으로 구현이 가능했다.
- 수강자들이 베이스가 다양하기 때문에 첫번째 협업에 목표가 있었다
- role
  - [[git]] 을 사용해 보지 않은 분들이 대다수라, 단순하게 topic -> merge commit, 리뷰없이 운영했다.
  - ui 가 없으면 업무 진행이 안될 것 같아, 가장 마지막에 남은 task 이기도했한 ui와 함께 각자 함수만 작성하면 통하도록 스켈레톤을 구현했다.
    ```mermaid
    flowchart
      user -.input.-> gradio
      python <--serve--> gradio
      python <-.api.-> llm
    ```
  - 대충 살붙여지면 아래와 같은 구조
    ```mermaid 
    flowchart LR
      user -.input.-> gradio
      python <--serve--> gradio
      python -."1. embed query".-> solar_embed_api
      solar_embed_api -."2. embedded query".-> python
      python --"3. retrive embedded query"--> retriver
      retriver --"4. retrived documents"--> python
      python --"5. create template with retrived documents"--> template_creator
      template_creator --"6. template"--> python
      python <-."7. llm query".-> solar_api(llm)
      solar_api(llm) <-."8. response".-> python
      python --"9. save response"--> history
    ```
    - 그래프에는 표현하지 않았는데 history 는 template 이 생성되는 타이밍에 함께 주입되어 llm query 에 포함된다
- 특정 도메인의 llm 서비스의 현존하는 방법을 알 수 있는 시간이 이었다.

## link
- [[upstage-ai-lab]]
- [[upstage-ai-lab-day-19]]
- [[upstage-ai-lab-day-28]]
