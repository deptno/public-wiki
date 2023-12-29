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

## history
- `0.29.0` 을 기준으로 스킨 적용 방식이 방식 변경
- `0.30.0` 을 바로보는 디렉토리가 바뀌고 yaml 스펙이 재정이 되었다
  - `yml` -> `yaml` 로 파일도 변경되어 헬이다 [[date:2023-12-29]]
- 아직 0.28.2 쓰는 중

## skin
- `$XDG_CONFIG_HOME/k9s/[context]_skin.yml` 위치에 파일을 만들고 skin 설정을 적용하면 해당 컨텍스트의 스킨이 적용된다.
- 0.29.0 버전에서 방식이 변경됨 0.29.1 에서 현재 신규 설정이 제대로 동작하지 않음 [[diary:2023-12-09]]
  - 다운그레이드는 [[brew]] 참조

## nodeShell
+ https://k9scli.io/topics/shell/
- `shellPod` 기능이 있는데 떠있는 컨테이너에 접속을 하는 개념이다
- [X] neovim 설정상태로 [[docker]] 이미지를 생성해서 붙을 수 있는지
  - shellPod 에서 셋업된 컨테이너를 띄우면 될 것으로 보인다
- `nodeShell` 의 개념은 해당 `node` 자체에 접속을 허용하는 것이며 원하는 파드를 통해서 띄우게된다
  - `/host` 를 통해서 해당 노드의 파일 시스템이 마운트 된다
  - `chroot /host` 를 통해서 해당 노드에 `ssh` 와 같은 형태로 접속하는 것을 갈음할 수 있다

## plugin
- [[metrics-server]] 를 설치하면 노드에 cpu, mem, pod 등의 정보를 얻어올 수 있다

## [[error]]
### pod 접근시 바로 종료되는 문제
- [[kubernetes#error]] [[pod]] 참고

### < Ruroh? `list access denied for user on "default":v1/pods 
- 0.30.0 부터 `v1/pods` 의 get 권한이 필요한 것으로 보인다 
- `pods` 는 원래 그룹([[kubernetes]], `1.26.0` 기준)이 없는데 이 부분이 업데이트 된 것인지 확인이 필요

## link
- [[kubernetes]]
- [[lens]]
- [[metrics-server]]
- [[popeye]]
- [[docker]]
- [[pod]]
