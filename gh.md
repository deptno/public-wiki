# gh|github-cli

## 명령어
```sh
gh repo view -w
gh pr list
gh pr view xxxx
gh pr list --json number,title -q '.[] | [.number, .title] | @csv' | xsv table
gh pr list -S HASH # 커밋이 포함된 pr
gh pr list -S HASH -s merged
gh pr list -S --draft=true # 드래프트인 pr만 노출
gh pr list -S 'draft:false' # 드래프트가 아닌 pr만 보여준다
gh pr list -S 'draft:false' -s merged
gh dash # dashboard
gh actions # action 관련 명령어
gh config list # config 목록 확인
gh config set editor "vim -c 'set filetype=gitcommit'" # 아래 neovim 섹션 참조
```

### gh-dash
```sh 
gh dash
```
대시보드 형태로 열리는데 [[path|~/.config/gh-dash/config.yml]] 에 설정을 해두면 여러 PR을 편리하게 관리가 가능하다

## 다중 계정
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

## template
```sh
gh pr list \
  --json author,baseRefName,comments,number,title,mergeable,createdAt,isDraft,state \
  --template \
'{{tablerow "#" "createdAt" "draft" "state" "base" "mergeable" "author" "title"}}
{{range .}}
{{tablerow .number .createdAt (.isDraft | autocolor "green") .state .baseRefName .mergeable .author.login .title}}
{{end}}'
```

## [[neovim]]
- pr 작성중에 `#18` 등을 통해서 pr 혹은 issue 에 대한 정보 및 자동완성 제공
  + https://github.com/petertriho/cmp-git
  - pr 생성등에서 `nvim-cmp` 와 함께 해서 지원을 받을 수 있다.
  - 이를 위해서는 `gh pr create` 등의 명령어를 사용할 때 filetype 이 설정되어 있어한데 기본적으로 `markdown` 이 지정된다
  - `git_cmp` 설정에서 `filetypes` 에 `gitcommit` 이 포함된 것(기본설정) 확인 필요
```sh
gh config set editor "vim -c 'set filetype=gitcommit'" # assist 도움을 받기 위해서 파일타입이 필요한 경우 지정
gh config list # 확인
```

## link
- [[direnv]]
- [[github]]
- [[jq]]
- [[xsv]]
- [[neovim]]
