# seo
- ogtag debug
  + https://en.rakko.tools/tools/9/
    + [[dev-tools]]
- og, twitter - 둘다 정의한다
- canonical - 중복 페이지로 원본이 있는 경우 명시한다
- json-ld
  - 모든 페이지에 작성하는게 권장된다
  - [[google]] 검색 결과가 상단에 표시되는 **rich snippet** 확보에 유리하며 seo 평가에 도움이된다

## [[nextjs]] 사용시 알아 둘점
- 버전 14를 기반으로 metadata 정의가 상위 정의와 함께 머지된다
- 중복 정의는 하위 정보로 대체된다
- **루트 키**를 기준으로 대체되므로 `openGraph.title` 속성을 재정의하는경우 `openGraph` 자체가 재정의된다

## link
- [[nextjs]]
- [[dev-tools]]
