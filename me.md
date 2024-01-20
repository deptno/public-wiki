# me
> 정리되지 않음 기록

- 개발 철학
- 읽은 책
  - 
- 언어
  - typescript
  - lua
  - rust
  - 책 한권 swift
    - clojure
    - go
    - c
- 관심 분야
  - editor, vim, 자동화, workflow
- 퇴사
  + [[about-me]] 에 정리
  1. ebook 책 같은거 읽다가 ebook 업체 취업
  2. dynamon 이전에 sns + blog 등을 통합한 이력서 서비스
- 만들고 싶은거
  - editor
- [X] 만들었던 것
  + [[about-me]] 에 정리
- 가진 기술
  - [[kubernetes]]
  - [[typescript]]
  - [[nextjs]]
  - [[css]]
    - [[tailwindcss]]
    - tachyon
  - [[intellij]]
  - [[neovim]]
  - [[db]]
    - [[postgresql]]
      + https://github.com/deptno/pg-toolbox/tree/master/packages/asql
        - asql, orm 이 불편해서 tagged template string 기반으로 sql이 보이도록 구현
    - dynamodb
      - project
        - tubemon.io
        - googit.io
        - googit.co
        - yiguana
  - graphql
    - lib
      - graphql-toolbox
      - dataloader-toolbox
        + https://github.com/deptno/dataloader-toolbox
          - graphq n+1 문제해결을위해 data-loader를 사용하는데 request 당 하나로 캐시되는 
            라이브러리, react 18 에서 도입된 cache 와 유사한 개념
          - [X] n+1 문제가 지금와서 잘생각이 안남 정리 필요
  - uml
    - [ ] plantuml
    - [X] mermaid
  - git 
    - connect history
      - repository 이동되면서 git history 를 잃어버림
      - 기존 repository 의 히스토리를 가지고와서 현재 이동되어 히스토리가 없는 커밋에 붙이는 과정
      1. 단순한
        - re init
        - commit current state to commit
        - add old repository to remote
        - rebase old/branch
      2. 추가적으로 유저가 필요한 경우 적용시, push가 이미 진행된 레포에 리베이스가 불가능할시
        - 레포작업 후, 작업기록 기억 못함
        - 적용시에는 `git fetch origin 'refs/replace/*:refs/replace/*'`
- 기술
  - srp 등
  - rxjs
    - tubemon.io 크롤러가 이걸로 만들어짐
  - [[functional]]
  - [[node]]
  - bundle
    - [ ] webpack vite esbuild 정리 필요

## link
- [[about-me]]
- [[home]]
- [[diary/index]]
