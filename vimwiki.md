# vimwiki

문서: https://github.com/vimwiki/vimwiki/blob/master/doc/vimwiki.txt

## reference
- diary:yyyy-MM-dd 다이어리 참조

## ~~tag~~
> 태그는  느려서 그냥 '[ripgrep](ripgrep)' 을 쓰는게 빠름
```vim
:h vimwiki-syntax-tag
```

  - `VimwikiRebuildTags`
  - vimwiki-ooption-auto_tags

## problem
- `Tab` 이 매핑되어있는 `VimwikiNextLink` 가 `ctrl+i`(forward move) 와 충돌한다, [[vim]] 만의 문제는 아님
- filetype 이슈 -> [tree sitter](tree-sitter) 지원을 통해서 해결
  - `vimwiki` 일때
    - [tree sitter](tree-sitter) 의 지원을 받지 못함
  - `markdown` 일때
    - vimwiki 에서 지원하는 `VimwikiTableAlignW` 같은 명령어를 지원받지 못함

## link
- [[vim]]
- [[telekasten]]
- [markdown](markdown)
- [tree sitter](tree-sitter)
