# git-bundle

> git 을 관리되는 커밋, 트리, 브랜치, 태그 를 하나의 파일로 번들링하며 이후 이를 기반으로 `clone`, 혹은 기존 코드 업데이트(`unbundle`) 가능하다
> untracked files, stash 목록은 포함되지 않는다. 백업  및 복구에 유용

## usage
- project directory 에서 
```sh 
git bundle create [name.bundle] --all # 전체 목록을 번들링
git bundle create [name.bundle] HEAD # 해당 브랜치만 번들링
git clone [name.bundle] # [name] 디렉토리로 클론됨
git clone [name.bundle] -b main [clone_directory_name]
```

## link
- [[git]]
- [[git-clone]]
- [[git-unbundle]]
- [[backup]]
- [[archive]]
