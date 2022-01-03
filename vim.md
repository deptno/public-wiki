# vim

```sh
man vim
```
```vim
" ex command 결과보기
:messages
``````

## 열기 옵션
```sh
$ vim -c "24" file.txt
```
- `-c` : 첫 번째 파일이 열린 후 ex 명령어로 실행
- `-R` : 읽기 전용으로 열기
- `+[LINE_NUM]` : 해당 라인에서 열기
- `+/[TEXT]` : 텍스트를 검색하여 열기
- `-` : stdin 으로 입력을 받는 경우`|` 사용으로 받는 경우등에 사용된다.
- `-r` : 스왑파일 리스트 출력
- `-O` : 여러 파일을 제공하면 창을 분할하여 보여준다. 수직분할
  ```
  $ vim -o $(git status -s | awk '{print $2}') # 상태가 변한 애들 모두 연다
  ```

## [[ex-mode]]
- move 0 : 가장 위로 이동
- move +1 : 현재 라인 아래로 이동
- move -1 : 현재 라인 위로 이동
- copy 0 : 가장 위로 복사
- copy +0 : 현재 라인 아래에 복사 yyp
- copy +1 : 현재 라인 아래에 복사

`copy` 는 `t` 로 사용가능 `move` 는 `m` 로 사용가능.

### vim <-> os
#### 파일명 복사
`+` 레지스터가 클립보드(mac)인걸 이용하여 아래와 같이 전달 한다.
```vim
" 현재 파일 경로
:let @+ = expand("%")
" 절대경로
:let @+ = expand("%:p")
" 디렉토리 까지
:let @+ = expand("%:h")
```

### 변수
```vim
" 확인
:set clipboard?
" prepend
:set clipboard^=unnamed
" append
:set clipboard+=unnamed
" override
:set clipboard=unnamed
```

### 워킹 디렉토리 변경
```vim
" 현재 파일의 디렉토리로 변경
cd %:p:h
```

## [[normal-mode]]
- `gf` : 현재 윈도우에서 파일을 따라간다. 단 `@` 가 있는 경우 [[visual-mode]] 에서 따라가도록한다.

## key-binding
### <Plug> [[question]]
플러그인에서 바인딩 전용으로 내보내는 것 같기도??
- :VimwikiToggleListItem<CR>
- <Plug>VimwikiToggleListItem

### 특정 파일 타입에서만 바인딩을 원하는 경우
```sh
autocmd FileType vimwiki nmap x <Plug>VimwikiToggleListItem
```

## 패턴 검색
> `/` 로 시작

```vim
" 대소문자 구분 안함
:set ignorecase
" 대문자가 포함된 경우 구분 / 포함되지 않은 경우 구분 없이 검색
:set smartcase
```

```vim
# regexp
/\v[SEARCH_TEXT]
# 일반 문자열 검색
/\V[SEARCH_TEXT]
# 대소문자 구분 안함
/\c[SEARCH_TEXT]
# 대문자 구분
/\C[SEARCH_TEXT]
```

## plugins
- [[vimwiki]]
- [[defx]]
- [[floatterm]]
- [[taskwiki]]
- [[vim-surround]]

## releated
- [[neovim]]
