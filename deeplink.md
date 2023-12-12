# deeplink

> 앱 내의 컨텐츠에 도달(deep)하기 위한 링크, 웹은 기본적으로 딥링크, spa 가 아닌이상
+ https://www.branch.io/ko/resources/blog/유니버설-링크-uri-스킴-앱-링크-및-딥-링크-무슨-차이가/

- deeplink
- gen 1
  - uri scheme
  - cons
    - 앱이 깔려있지 않은 경우
    - 두개이상의 앱이 하나의 scheme 을 제공하는경우
- gen 2
  - ios: universal-link
    - 앱을 바로시작
    - 없으면 url
  - android: applink

## 방식
> 잘 정리된 문서가 있으니 아래 링크 참조
+ https://medium.com/prnd/딥링크의-모든것-feat-app-link-universal-link-deferred-deeplink-61d6cf63a0a5

### uri scheme
`[scheme]://path/to/link`

- 스킴은 자유롭게 등록 가능
- 동일 스킵을 지정한 앱들이 있는 경우에 uri scheme 이 열린경우
  - [[android]] 선택
  - [[iOS]] 마지막에 설치된 앱이 선택됨
- *단점* 동일 스킴을 사용하는 앱을 만들어서 설치 후 어뷰징 가능

### universal link / applink
`https://domain/path/to/link`

- url과 동일하게 생겼다
- 앱을 등록할때 해당 도메인의 소유주임을 증명
- 해당 domain 이 브라우저로 열릴 때,  도메인 소유주앱이 있는 경우 실행
  - 없으면 웹링크가 그대로 열림
- *단점* 브라우저에 직접 복사 입력하는 경우 동작하지 않
- *단점* 웹뷰, 인앱브라우저, 서드파티 브라우저에서 동작이 일정치 않다

### deferred deeplink | 디퍼드 딥링크
> deeplink 를 오픈해보고 에러가 났을때 다시 앱스토어로 리다이렉트 시키는 방향으로 구현된것이 아닐까하는 생각

- 앱이 설치되어 있으면 앱을 실행 
- 앱이 설치되어 있지 않으면 설치페이지로 유도
  - 이후 앱이 실행되면 딥링크를 실행
- 기본적으로 웹 url 이고 판단에 따라서 앱이 설치 유무에 따라 동작이 달라지는 컨셉
- 솔루션
  - firebase dynamic link
  - [[appsflyer]] one ink
  - branch
  - adjust

## link
- [[universal-link]]
- [[applink]]
