# bun
[[javascript]] runtime

- [[typescript]] 지원

## config
`.bunfig`

- home 디렉토리 밑에 있으면 글로벌로 사용
- package.json 위치에 만들어서 local 로 사용

## package.json 옵션
### overrides
- [[yarn]] `resolutions` 와 같은 옵션으로 개별적으로 다른 가지는 패키지들에 대해서 버전을 픽스한다,

## [[error]]
### `bun.lockb` 가 꼬이는 문제
- [[react-native]] 버전업 중 문제가 생겼을 때 `bun.lockb` 제거 하고 다시 인스톨하니 동작하는 케이스가 있었음
  + [[diary:2023-12-25]] 참고

## patch-package
> bun 은 공식적을 patch-package 를 지원하지 않는다
```sh
git diff path/to/as-is path/to/patched > patches/[package-name]+[version].patch
```
- 위 명령어로 로 패치 파일을 생성한다, 경로는 프로젝트 루트의 `patches/` 밑이다
- `package.json`에 "postinstall": "bunx patch-package" 를 넣어서 `install` 이 끝난 후에 패치패키지를 적용하도록한다

## link
- [[node]]
- [[javascript]]
- [[typescript]]
- [[zig]]
