# grep

## 옵션
- o : 라인이 아닌, 매치된 단어만 표시
- h : 헤더, 파일명등을 표시하지 않음
- E : 정규표현식 매치 사용
- n : 라인 표시
- r : 특정 폴더 하위로 검색
- v : 제외
- `color` 패턴에 일치하는 경우 컬러를 부여, `color=always` 와 같이 쓰는 경우 pipe 를 통해 [[less]] 에서 컬러유지가 가능함

## 모든 라인 출력 + 하이라이트

```sh
grep --color -E 'KEYWORD|$' -r [DIR]
```

[[ansi-color]] 변경시에는 [[env]] 설정 필요 `GREP_COLOR`
```sh
GREP_COLOR='35;47' grep --color -E 'KEYWORD1|KEYWORD2|$' [FILENAME]
```

> GREP_COLOR
> It is deprecated in favor of GREP_COLORS, but still supported.
https://www.gnu.org/software/grep/manual/html_node/Environment-Variables.html

grep 을 분리해서 여러번 파이프로 연결하는 경우 각각 다른 색상 부여도 가능함

## 자매품
- [[egrep]] - grep -E, 패턴을 escape 없이 정규식으로 처리하는 경우
- [[fgrep]] - grep -F, 정규식을 사용하지 않고 처리하는 경우

## link
- [[less]]
