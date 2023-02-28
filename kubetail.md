# kubetail

https://github.com/johanhaleby/kubetail

여러 파드의 로그를 함께 볼 수 있다.

+ 2023-02-23 
  conrjob 등 새로 생성되는 로그를 볼수 없어서 [[stern]] 으로 갈아탐

## install
```sh
$ brew tap johanhaleby/kubetail && brew install kubetail
```

난 zsh 로 설치함

## usage
```sh
kubetail text # match
kubetail -n # current namespace pods
kubetail -n [namespace]
```

## related
- [[kubernetes]]
- [[stern]]
