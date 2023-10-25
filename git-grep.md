# git-grep

> `git log --grep` 달리 커밋 메시지가 아닌 실제 코드에 대한 검색
+ https://stackoverflow.com/questions/2928584/how-to-grep-search-committed-code-in-the-git-history

- HEAD 검색은 [ripgrep](ripgrep) 으로 대체가 가능
- 

## usage
```sh
# git grep [REGEXP]
git grep -pPn [REGEXP]
git grep -pPn [REGEXP] [COMMIT] # 해당 revision 에서 검색 수행
```

### options
- `-P` : perl compatible regular expression
- `-n` : 라인넘버를 포함하여 출력
- `-p` : 함수 이름을 포함
- `-F` : fixed string 형태로 검색한다. regexp 가 아닌겨우

## link
- [[git]]
- [[git-log]]
- [ripgrep](ripgrep)
