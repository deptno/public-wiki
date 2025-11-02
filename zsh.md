# zsh
- https://linuxhint.com/if-else-conditionals-zsh-script/

## 여러 라인 입력
- 명령어 뒤에 `\` 를 붙이고 공백없이 엔터치면 가능
- 이미 붙여 넣은 긴 명령어를 가독성 개선하고자 한다면 `ctrl + x`, `ctrl + e` 를 순차로 누르면 editor 를 통해서 수정 가능

## shell-script
### string
```sh
if [[ "A" = "" ]]; [[then]]
fi
if [[ "A" != "" ]]; then
fi
if [[ -z "$TMUX" ]]; then
fi
```

- n : non-zero length
- z : length

## plugins
- [[zsh-syntax-highlighting]]
- [[kubectl]]

## alias
### git
- gpristine - [[git-clean]] [[git-reset]] 조합으로 처리하던 것을 한번에 정리한다. [[git-clone]] 을 상태와 동일하게 돌린다.

## link
- [[shell-script|쉘스크립트]]
