# k9s

터미널 기반 k8s 매니징 툴
```sh
brew install k9s
```

vim 에서와 같이 `:` 로 이동이 가능하다.

| command | description             |
| ------- | ----------------------- |
| :q      | 종료                    |
| :dp     | deployment              |
| :svc    | service                 |
| :sec    | secret                  |
| :ns     | namespace               |
| :po     | pod                     |
| :pv     | persistent volume       |
| :pvc    | persistent volume clame |
| :cm     | config map              |

## skin
`$XDG_CONFIG_HOME/k9s/[context]_skin.yml` 위치에 파일을 만들고 skin 설정을 적용하면 해당 컨텍스트의 스킨이 적용된다.

## related
- [[kubernetes]]
- [[lens]]
