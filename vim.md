
```sh
man vim
```
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

## ex
- move 0 : 가장 위로 이동
- move +1 : 현재 라인 아래로 이동
- move -1 : 현재 라인 위로 이동
- copy 0 : 가장 위로 복사
- copy +0 : 현재 라인 아래에 복사 yyp
- copy +1 : 현재 라인 아래에 복사

`copy` 는 `t` 로 사용가능 `move` 는 `m` 로 사용가능.

## plugins
- [[vimwiki]]
- [[defx]]
- [[floatterm]]
- [[taskwiki]]

## releated
- [[neovim]]

