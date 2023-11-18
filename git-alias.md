# git-alias

- git gone
- git dmb
  + https://github.com/deptno/.config/commit/b7a71c4aab744c734c0984ff573ec143b9e50e30
```sh 
git for-each-ref | grep 'commit\trefs/heads' | grep -v "$(git rev-parse @)" | awk '{print $3}' | sed 's|refs/heads/||' | xargs -I {} sh -c 'git branch --contains {} | grep "* " > /dev/null && [ $? -eq 0 ] && git branch -d {}' 
```

## link
- [[git]]
- [[alias]]
