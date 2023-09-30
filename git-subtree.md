# git-subtree

- [[git]] 으로 관리되고 있는 파일들에 다른 레포지터리에 있는 특정 브랜치를 **커밋 내역과 함께 가져올 때** 사용 가능하다.
- 두 레포지터리를 하나로 합치는 결과를 가져온다

```sh 
git subtree add --prefix=lua/custom git@github.com:deptno/neovim-config-for-nvchad.git main
```

`--prefix` 는 가져올 경로가 된다. 즉 특정 폴더로 git@github.com:deptno/neovim-config-for-nvchad.git 레포지터리의 `main` 브랜치의 커밋을 가져오게된다


## link 
- [[git]]
- [[git-submodule]]
