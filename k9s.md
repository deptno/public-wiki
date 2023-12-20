# k9s

터미널 기반 k8s 매니징 툴
```sh
brew install k9s
```

vim 에서와 같이 `:` 로 이동이 가능하다.

| command             | description                          |
| -------             | -----------------------              |
| :q                  | 종료                                 |
| :dp                 | deployment                           |
| :svc                | service                              |
| :sec                | secret                               |
| :ns                 | namespace                            |
| :po                 | pod                                  |
| :pv                 | persistent volume                    |
| :pvc                | persistent volume clame              |
| :cm                 | config map                           |
| :pu[lses]           | show graph                           |
| :popeye             | show [[popeye]]                      |
| :xray [type] [name] | show hierachy                        |
| :sd                 | show dumps (log 에서 ctrl-s 로 저장) |

- `:secret` -> `x` decrypt

## skin
- `$XDG_CONFIG_HOME/k9s/[context]_skin.yml` 위치에 파일을 만들고 skin 설정을 적용하면 해당 컨텍스트의 스킨이 적용된다.
- 0.29.0 버전에서 방식이 변경됨 0.29.1 에서 현재 신규 설정이 제대로 동작하지 않음 [[diary:2023-12-09]]

## shell pod
+ https://k9scli.io/topics/shell/
- shell pod 기능이 있는데 떠있는 컨테이너에 접속을 하는 개념이다
- [ ] [[@todo]] neovim 설정상태로 [[docker]] 이미지를 생성해서 붙을 수 있는지

## plugin
- [[metrics-server]] 를 설치하면 노드에 cpu, mem, pod 등의 정보를 얻어올 수 있다

## [[error]]
### pod 접근시 바로 종료되는 문제
- [[kubernetes#error]] `pod` 참고

## link
- [[kubernetes]]
- [[lens]]
- [[metrics-server]]
- [[popeye]]
- [[docker]]
