# deptno.dev

개인 기억 보조 위키

## todo [[@todo]]
- [X] [[deptno.dev]] 에서 push event를 받아서 자체 재시작(업데이트가 아닌)하도록 설정 - [[rust]]
  - [X] process.exit + livenessProbe 로 process 를 재시작할 뿐 pod 나 container 를 재시작할 수 없음
  - [X] 결국 [[webhook]] -> [[kubernetes-api]] 를 통해 rollout 을 하는 방향으로 수정되어야함
  - [X] 생각해 보니 서버가 아닌 wiki 의 레포가 일반 레포가 아닌 wiki repo 여서 이벤트를 ~~받을 수 없음~~
    - 당분간 수동
    - [X] gollum webhook event 가 있어서 위키도 이벤트를 받을 수 있음
      + https://github.com/deptno/deptno.dev/commit/7f2d0cd8a65973b35476f131dfe13442ae468d04

## link
- [[webhook]]
