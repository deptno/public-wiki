# mermaid

- text to diagram
- plantuml 과 비슷한 일을 하지만 github 에서 지원
- mermaid-cli 를 통해서 로컬 이미지 생성이 가능

## gant
### syntax
```text
gantt
  dateFormat  YYYY-MM-DD
  axisFormat  w%W
  title       TEXT
  excludes    weekends

  section     SectionName
  task1       :KEYWORD, KEYWORD, ID, from, to
  task1       :KEYWORD, KEYWORD, after ID, to
```
- dateFormat 은 input 타입으로 출력과는 다르다
- axisFormat 이 날짜에 대한 출력 패턴(x축)
- excludes 를 통해서 패턴을 프로젝트 일정에서 제거한다
- section 은 세로축 row 가 된다
- task 는 section 안에서의 column 이 된다

### keywords
- active
- done
- crit
- after

## link
- [[plantuml]]
- [[mermaid-cli]]
