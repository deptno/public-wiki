# watchman

```sh
brew install watchman
```
[[react-native]]의 `Error: EMFILE: too many open files, watch` 에러를 잡는데 사용된다.
시스템의 fd 제한을 우회하는데 아마도 symlink 를 건너 띔으로써 처리하는 것이 아닌가 한다. [[todo]]

```sh
$ ll -T ~/Library/Developer/Xcode/DerivedData/ | wc -l
  131025
$ sysctl kern.maxfiles
491520
$ sysctl kern.maxfilesperproc
245760
```
ll

어디까지가 watch의 범위인지는 확인해보지 않았으나 커널의 환경설정을 넘어선다.
- [ ] @todo node 의 fs.watcher api 가 symlink 에 대해서 어떻게 동작하는지 알아보자

## link
- [[react-native]]
- [[sysctl]]
