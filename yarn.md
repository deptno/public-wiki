# yarn

berry 기준

```sh
yarn info _package_name_
yarn info _package_name_ version
yarn patch-package
yarn set version berry
```

## workspace, monorepo, pnp
**symlink 는 폴더이름이 아니라 `package.json#name` 이 사용된다.**
`yarnrc`에 `workspaces-experimental false` 를 추가해서 workspace 를 off 할 수 있다.
이 경우에 퍼블리싱이 잘 되는지도 확인해 볼 것 [[@todo]]

yarn add --force @workspace/package [package_name]

```
yarn set version berry
yarn init -w
cd packages
mkdir lib
cd lib
yarn init -y
```
package.json
```
{
  "name": "tmp",
  "packageManager": "yarn@3.2.0",
  "private": true,
  "workspaces": [
    "packages/*"
  ]
}
```
[[packages/lib/package.json]]
```
{
  "name": "papago",
  "type": "module",
  "dependencies": {
    "node-fetch": "^3.2.0"
  }
}
```

### node_modules
.yarnrc.yml
```
yarnPath: .yarn/releases/yarn-3.2.0.cjs
nodeLinker: node-modules
```
혹은
```
yarn config set nodeLinker node-modules
```
- nodeLinker: 를 통해 node_modules pnp 에서도 사용 가능하다.
`node_modules` 이 없으면 2022-03-14 현재도 [[webstorm]] 에서 제대로 사용이 불가능하다.


### nohoist
- https://classic.yarnpkg.com/blog/2018/02/15/nohoist/
/package.json
```
{
  "workspaces": {
    "packages": [ "packages/*" ],
    "nohoist": [ "**/react-native" ]
  }
}
```


## related
- [[node]]
- [[npm]]
