## 모든 라인 출력 + 하이라이트
```sh
grep --color -E 'KEYWORD|$'
# 색상 추가
```

[[ANSI color]] 변경시에는 [[env]] 설정 필요 `GREP_COLOR`
```sh
GREP_COLOR='35;47' grep --color -E 'KEYWORD1|KEYWORD2|$'
```

> GREP_COLOR
> It is deprecated in favor of GREP_COLORS, but still supported.
https://www.gnu.org/software/grep/manual/html_node/Environment-Variables.html

