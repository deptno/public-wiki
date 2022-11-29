# multipass

canonical 에서 드라이브하는 vm, 우분투 셋업을 쉽게 할 수 있다.

```sh
brew install multipass
multipass find
multipass ls
multipass stop [name]
multipass delete [name]
multipass purge [name]
multipass info [name]
multipass launch [name by find command]
multipass mount $HOME [name of vm]{{:/pass}} # https://multipass.run/docs/share-data-with-an-instance
```

## related
- [[kubespray]]
