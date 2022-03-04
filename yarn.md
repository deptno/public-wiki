# yarn

berry 기준

```sh
yarn info _package_name_
yarn info _package_name_ version
yarn patch-package
yarn set version berry
```

## workspace, monorepo, pnp
```
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
packages/lib/package.json
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
- nodeLinker: 를 통해 node_modules pnp 에서도 사용 가능하다.


## related
- [[node]]
- [[npm]]
