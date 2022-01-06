# grep

##
- `color` 패턴에 일치하는 경우 컬러를 부여, `color=always` 와 같이 쓰는 경우 pipe 를 통해 [[less]] 에서 컬러유지가 가능함

## 모든 라인 출력 + 하이라이트

```sh
grep --color -E 'KEYWORD|$'
```

[[ANSI color]] 변경시에는 [[env]] 설정 필요 `GREP_COLOR`
```sh
GREP_COLOR='35;47' grep --color -E 'KEYWORD1|KEYWORD2|$'
```

> GREP_COLOR
> It is deprecated in favor of GREP_COLORS, but still supported.
https://www.gnu.org/software/grep/manual/html_node/Environment-Variables.html

grep 을 분리해서 여러번 파이프로 연결하는 경우 각각 다른 색상 부여도 가능함

## related
- [[less]]
