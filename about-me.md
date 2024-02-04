# about-me
- deptno@gmail.com

# 커리어
### 2021-05 ~ 현재: 직방, 프론트엔드 파트 매니저
- [[react-native]] 기반 직방앱 개발 매니징, 원룸, 오피스텔, 빌라 파트

### 2019-06 ~ 2021-05: 알스퀘어, 팀장
- 사내 back-office 개발, 프론트엔드로 시작하여 백엔드까지 전체 타입스크립트 + 노드로 마이그레이션
  - [[csharp]] -> [[node]] + [[graphql]] + [[dataloader]] 마이그레이션
- elk + beat 기반의 [[log]] 모니터링 시스템 생성
- [[kubernetes]] + [[helm]] 기반의 배포환경 구성

### 2016-04 ~ 2018-03: 알스퀘어, 프론트엔드 개발팀장
- 회사 서비스 홈페이지 개발
- 사내 back-office, html 기반 PDF 생성
- [[serverless]], ECS 기반의 배포환경

### 2011-06 ~ 2015-07: Humax, 대리
- 2011년, [[typescript]] 베이스의 SPA 로 구현된 세탑박스 UX 구현
- 영국 및 독일 제품에 포함되어 출고, OTA 지원 및 타 서비스(BBC 등)의 앱 런칭 가능
- [[electron]] 과 유사하게 임베디드 브라우저에 커스텀 오브젝트를 구현하여 미들웨어와 통신하는 방식
+ 당시 베이스로 구현된 라이브러리
  + **깃허브** https://github.com/controlsts/controls.ts
- 제품 영상(순서대로 영국, 독일, 영국 제품)
  + **유튜브** https://www.youtube.com/watch?v=RfhtVHHoVcE
  + **유튜브** https://www.youtube.com/watch?v=GZtnesjkVyo
  + **유튜브** https://www.youtube.com/watch?v=vN850-Dy6Yw

# 개인 프로젝트
### 살지로
- 커뮤니티 게시판 딜 글들을 모아 알림서비스 제공
- [[react-native]], [[next-auth]]
+ **홈** https://app.salji.ro
+ **ios** https://apps.apple.com/kr/app/%EC%82%B4%EC%A7%80%EB%A1%9C/id6475012933
+ **android** 심사중
+ **개발 기록** [[wip]] [[diary:2024-01-25]]

### [[vimwiki]] 기반의 범용 프론트엔드
- 지식의 인덱싱을 위해서는 blog 가 아닌 wiki 가 필요하다는 판단, [[vimwiki]] frontend 구현
+ **서비스** https://deptno.dev
+ **깃허브** https://github.com/deptno/deptno.dev

### 튜브몬
- 유튜버 랭킹 + 커뮤니티 핫딜 모음
- [[aws]] [[serverless]] 서비스들로만 구현된 유튜브를 5년정도 서비스
- 그 이후 커뮤니티 핫딜을 서비스에 추가한며서 로컬 [[kubernetes]] 로 서비스 옮김
- 게시판은 이구아나를 기반으로 구현되어 [[dynamodb]] 를 사용
- [[postgresql]]
+ **서비스** https://tubemon.io

### 공인중개사 문제은행
- 시험문제 -> [[markdown]] 변환 이후 커스텀 렌더러를 통해서 모든 렌더링을 구현
- [[serverless]] 서비스들로만 구현 + [[dynamodb]], [[lambda]]+edge([[seo]]), cognito, iot(websocket)
- [[nextjs]], [[graphql]]
+ **서비스** https://googit.co

### 구깃
- 이력관리 + SNS link 를 통한 프로필, 이력서 제공 서비스
- [[serverless]] 서비스들로만 구현 + [[dynamodb]], [[lambda]]+edge([[seo]]), cognito
- [[nextjs]], [[graphql]]
+ **서비스**  https://googit.io
+ **개발 기록** https://googit.io/post/ap-northeast-2:c03f8bf0-992e-48a8-93b6-15787a0fc96f/public/googit/

### 이구아나
- [[dynamodb]] 기반의 서버리스 게시판 백엔드 + SDK
+ **테스트** https://yiguana.dev.googit.co
+ **깃허브** https://github.com/deptno/yiguana

### 다이나몬
- [[electron]] 으로 구현된 [[dynamodb]] 클라이언트
+ **깃허브** https://github.com/deptno/dynamon

### 지하철 물품보관함 해피박스 위치 검색
- [[github]] page 로 서빙, 노트북 들고 다니면서 산책하기 무릎아파서 구현
+ **서비스** https://deptno.github.io/map-subway-storage
+ **깃허브** https://github.com/deptno/map-subway-storage

# 개인 라이브러리
### dataloader-toolbox
- session 당 [[dataloader]] 래퍼 함수의 인터스턴스 유지하기 위한 session
- [[sql]] `IN` operator 가 함수 순서보장을 안하므로 서버에서 정렬하기 위한 sorter
+ **깃허브** https://github.com/deptno/dataloader-toolbox

### pg-toolbox
- 쿼리와 변수를 함께 보기위한 *tagged template literals* 기반한 sql 생성기
+ **깃허브** https://github.com/deptno/pg-toolbox/tree/master/packages/asql

# 개인 채널
- **유튜브**: https://www.youtube.com/@user-cl9cj5ty2v

# 개인 설정
### .config
- [[terminal]] 사용을 위한 개인 설정
+ **깃허브** https://github.com/deptno/.config

### [[neovim]] 설정
- 개인 설정 및 구현 플러그인
+ **깃허브** https://github.com/deptno/nvim
+ **깃허브** https://github.com/deptno/gx.nvim

### 자주 사용 툴
- [[alacritty]], [[chatgpt]], [[curl]], datagrip, [[fx]], [[gh]], [[jq]], [[k9s]], [[kubectl]], [[lazygit]], [[neovim]], [[raycast]], [[tmux]], [[webstorm]], [[zsh]]

## link
- [[me]]
- [[home]]
- [[diary/index]]
- [[wn.private:resume]]
