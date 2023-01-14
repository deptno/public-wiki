# alias

alias 를 생성할때 공백이 있으면 다음 명령어에도 alias 적용이 가능하다

예를 들자면 

```sh
alias kpf='kubectl port-forward'
sudo kpf awesome-svc 80:8000
```

이렇게 하면 `kpf` 의 경우 alias 이기 때문에 사용이 불가능하다
```sh
alias sudo='sudo '
sudo kpf awesome-svc 80:8000
```

sudo 뒤에 ` `이 포함되어 alias 가 생성되었기 때문에 다음 명령어도 이를 받아들인다

## related
- [[terminal]]
- [[sudo]]
