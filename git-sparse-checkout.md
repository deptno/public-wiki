# git-sparse-checkout

특정 위치만 `checkout` 받는 기능이다.
루트 디렉토리의 파일들은 자동으로 받아진다.
특정 디렉토리를 지정할 수 있다.

- `--no-tags`
- `--depth 1`

위와 같은 옵션을 함께 사용하여 더 최적화된 데이터를 `checkout` 할 수 있다.

- https://git-scm.com/docs/git-sparse-checkout
- CONE PATTERN SET
## 사용법
```sh
git init
# sparse checkout 기능을 enable 한다.
# --cone, CONE PATTERN SET 을 활성화한다.
git sparse-checkout init --cone
# 내려 받을 디렉토리를 선택한다.
git sparse-checkout set dir1 \
                        dir2 \
                        dir3/sub/path
git remote add origin git@github.com:USER/REPOSITORY.git
git fetch --no-tags --depth 1 origin BRANCH
git checkout --force BRNACH
```

## related
- [[git]]
