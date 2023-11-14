# neotest

```vim
lua require("neotest").summary.toggle()
```

## neotest-jest
+ https://github.com/nvim-neotest/neotest-jest

### [[error]] +[[wip]] +[[@todo]]
- monorepo 설정을 하는데 문제가 있음
  - readme 에 있는 에제로는 정규표현식이 `src` 가 두개 포함되는 경로를 지원하지 못함
    + 임시 해결 커밋 https://github.com/deptno/nvim/commit/566b348bfe727ba142323094333fabb7b73e2323
- 구조가 좋지 않음
- 많이 느림,  해당 라이브러리 때문인지는 확실하지 않음
- summary window
  - 결과가 성공임에도 아이콘은 실패로 표기
  - `o` 를 눌러서 output window 표시를 하면 syntax highlighting 이 되지 않음
- status sign 표시 안됨
- output panel 표시 안됨

## link
- [[neovim]]
- [[neovim-setup]]
- [[jest]]
- [[vitest]]
- [[bun]]
