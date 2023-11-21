# git-alias

- git gone
- git dmb
  + https://github.com/deptno/.config/commit/b7a71c4aab744c734c0984ff573ec143b9e50e30
```sh 
git for-each-ref | grep 'commit\trefs/heads' | grep -v "$(git rev-parse @)" | awk '{print $3}' | sed 's|refs/heads/||' | xargs -I {} sh -c 'git branch --contains {} | grep "* " > /dev/null && [ $? -eq 0 ] && git branch -d {}' 
```
- 최근 수정 파일
```sh 
ds =  ! "f() { git diff $(git log --since='30 days ago' --pretty=format:'%H' | tail -1).. --stat; }; f"
```
  + https://github.com/deptno/deptno.dev/blob/20a39fa2/apps/wiki/lib/getLastModifiedFiles.ts#L6-L10

## link
- [[git]]
- [[alias]]
