# webstorm

- inlay hint
  virtual text 와 같은 형태로 ghost 처럼 타입등의 힌트를 보여주는 기능
- `shift`x2 `Create Command Line Launcher` -> OK

## typescript
### auto import
[[yarn]] [[monorepo]] 에서 auto import 시에 타겟 모듈의 package.json.main 을 존중하지 않는 경우라면 해당 모듈의 `tsconfig.json` 에
`baseUrl` 을 수정한다
```json
  "baseUrl": "./src"
```

## plugin
[[intellij|##plugin]] 참조

## [[error]]

```sh
The application /Applications/WebStorm.app cannot be opened for an unexpected reason, error=Error Domain=NSCocoaErrorDomain Code=260 "The file “WebStorm.app” couldn’t be opened because there is no such file." UserInfo={NSURL=file:///Applications/WebStorm.app, NSFilePath=/Applications/WebStorm.app, NSUnderlyingError=0x600002290060 {Error Domain=NSPOSIXErrorDomain Code=2 "No such file or directory"}}
```
Webstorm -> Tools -> Terminal -> Run command using IDE(? [[@todo]])

## link
- [[intellij]]
- [[yarn]]
- [[pycharm]]
- [[jetbrains]]
