# yarn

berry 기준

```sh
yarn info _package_name_
yarn info _package_name_ version
yarn patch-package
yarn set version berry
yarn plugin import @yarnpkg/plugin-workspace-tools
```
## pnp
대충 zero install 모드가 되면서 package 가 zip 파일로 `.yarn/cache` 디렉토리안에 들어가게된다.
대신 `node_modules` 디렉토리가 없어지면서 에디터에서 문제를 일으켰는데 webstorm@2022.3.1 기준, 잘 동작

다른 에디터는 여전히 문제가 되는데 이를 sdk 형태로 함께 배포하고 있다
- `@yarnpkg/sdks`

```sh 
yarn dlx @yarnpkg/sdks vscode vim
yarn dlx @yarnpkg/sdks base # neovim >= 0.6 native lsp
```
### lnp 모드시 모듈 임포트(require)
#### node cli 에서 
모듈 임포트가 안되서 테스트하기가 짜증났는데 옵션 줘서 해보니 된다
```sh 
node -r .pnp.cjs
$
```
#### 파일에서
```sh 
yarn node [script.js]
```
## workspace, monorepo, pnp
- 2023-02-04 참조
  + https://velog.io/@projaguar/Yarn-3-MonoRepo-with-Typescript-Next.js-Nest-React-Native
  + https://velog.io/@projaguar/Yarn-Berryv3-Workspace1-workspace-설정
  + https://velog.io/@projaguar/Yarn-Berryv3-Workspace-2-Nest-설정
**symlink 는 폴더이름이 아니라 `package.json#name` 이 사용된다.**
`yarnrc`에 `workspaces-experimental false` 를 추가해서 workspace 를 off 할 수 있다.
이 경우에 퍼블리싱이 잘 되는지도 확인해 볼 것 [[@todo]]

```sh
yarn add --force @workspace/package [package_name]
```

```
yarn set version berry
yarn init -w
yarn plugin import @yarnpkg/plugin-workspace-tools
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

### typescript internal modules
모노레포에서 하나의 패키지에서 다른 패키지를 참조하려면 실제로는 빌드를 되게 하는 것이 정석이나 번거롭다.
패키지만 분리하고 함께 컴파이되도록 하려면 타입스크립트 `compilerOptions` 에 추가 설정이 필요하다

```json
  "baseUrl": ".",
  "paths": {
    "@project/package": [
      "packages/package/src"
    ],
    "@project/package/*": [
      "packages/package/src/*"
    ]
  }
```
`@project/package` 는 참조할 패키지의 `package.json:name` 과 같게한다

### node_modules
+ 2023-02-03
  아래 내용은 @yarnpkg/sdks 를 통해 해결이 가능하며 webstorm 도 현재 zero install 을 지원해서 유용하지 않다
  
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
  + 2023-02-13 현재는 nodeLinker 없이 사용 가능 
  
[[ci]]


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

## error
```sh
Unknown Syntax Error: Command not found; did you mean:
```
workspace tool 이 있어야지 가능
```sh
# https://yarnpkg.com/api/modules/plugin_workspace_tools.html
yarn plugin import workspace-tools
```
+ 2023-01-31 workspace package 가 prefix 를 가지게 된 것으로 보인다
```sh 
yarn plugin import @yarnpkg/plugin-workspace-tools
```
---
The remote archive doesn't match the expected checksum
```
➤ YN0000: │ Some peer dependencies are incorrectly met; run yarn explain peer-requirements <hash> for details, where <hash> is the six-letter p-prefixed code
➤ YN0000: └ Completed in 0s 623ms
➤ YN0000: ┌ Fetch step
➤ YN0013: │ @ptomasroos/react-native-multi-slider@https://github.com/zigbang/react-native-multi-slider.git#commit=22a0d52e2edb40edb552c4e010cd629bc2e68e9c can't be found in the cache and will be fetched from GitHub
➤ YN0013: │ multiple-cucumber-html-reporter@https://github.com/Seunghyum/multiple-cucumber-html-reporter.git#commit=e03908f469aa694900da40597ea31a9a0ca3d86f can't be found in the cache and will be fetched from GitHub
➤ YN0013: │ @ptomasroos/react-native-multi-slider@git+https://github.com/zigbang/react-native-multi-slider.git#commit=22a0d52e2edb40edb552c4e010cd629bc2e68e9c can't be found in the cache and will be fetched from the remote r
➤ YN0013: │ multiple-cucumber-html-reporter@https://github.com/Seunghyum/multiple-cucumber-html-reporter.git#commit=e03908f469aa694900da40597ea31a9a0ca3d86f can't be found in the cache and will be fetched from the remote rep
➤ YN0018: │ @ptomasroos/react-native-multi-slider@git+https://github.com/zigbang/react-native-multi-slider.git#commit=22a0d52e2edb40edb552c4e010cd629bc2e68e9c: The remote archive doesn't match the expected checksum
➤ YN0018: │ multiple-cucumber-html-reporter@https://github.com/Seunghyum/multiple-cucumber-html-reporter.git#commit=e03908f469aa694900da40597ea31a9a0ca3d86f: The remote archive doesn't match the expected checksum
➤ YN0000: └ Completed in 5s 348ms
➤ YN0000: Failed with errors in 5s 976ms
$ YARN_CHECKSUM_BEHAVIOR=update yarn 
```
- https://yarnpkg.com/advanced/error-codes#yn0018---cache_checksum_mismatch
  + 2023-01-06
    - 실제로 위 방식을 해보니 해결이 되지만 다른 환경에서는 에러로 인식했다. 오직 나만 다른 체크섬이 생기는 상태
    - https://github.com/yarnpkg/berry/issues/2399
    - version v3.3.0 이상에서 해결 -> `yarn` 으로 `yarn.lock` 업데이트가 끝난 후에는 버전을 다시 내려도 정상적으로 작동됨
```sh 
yarn set version 3.3.1
```

## related
- [[node]]
- [[npm]]
- [[ci]]
