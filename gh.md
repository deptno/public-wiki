# gh|github-cli

github cli 로 편리한 명령어 몇가지를 제공한다.
다중 계정을 [[env|GH_CONFIG_DIR]] 을 통해서 지원할 수 있다.
[[direnv]] 와 조합시 편히 사용이 가능하다.
[[zsh]] 플러그인도 존재하므로 넣어두면 바로 활성화 된다. 
```sh
plugins=(
  ...
  direnv
)

```

```sh
 ~
│  .envrc
├──  deptno
│  ├──  deptno.github.io.wiki
└──  _external
   │   .envrc
   └──  rustlings
```
위와 같이 `.envrc` 파일이 다중으로 존재하는 경우 모든 부모의 환경을 상속받게 된다.
때문에 `external` 디렉토리에 들어오면 해당 디렉토리의 `.envrc` 를 통해서 부모의 [[env|GH_CONFIG_DIR]] 변수를 설정해서
추가적인 로그인 정보를 제공하면 특정 폴더에서 다른 계정으로 사용이 가능하다.

## related
- [[direnv]]
- [[github]]
