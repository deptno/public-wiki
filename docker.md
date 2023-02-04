# docker

컨테이너

```sh
brew install --cask docker 
```

## build
```sh
docker build . -f [Dockerfile.custom]
docker build . --no-cache # 결과 캐싱하지 않음, stdout 출력 필요시에도 사용
docker build . --pull # pull 옵션을 통해서 항상 리모트 이미지를 사용할지 결정이 가능
docker build . --progress=plain # 기본적으로 프로그레스 텍스트(변하는 텍스트)를 보여주는데 이를 plain text 를 출력하는게 좋음
```

## error
container 가 실행중에 아래와 같은 형태의 에러를 낸다면 architecture 문제
즉 arm64 로 빌드된 이미지가 amd64 에서 실행된 경우
```sh
exec /whoami: exec format error                                                                                                              │
```

아래와 같은 형태로 플랫폼을 지정해서 pull 이 가능하다
```sh
docker pull --platform [linux/amd64] [image-name]
```
---
```sh
STEP 9/9: RUN yarn --immutable
➤ YN0000: ┌ Resolution step
➤ YN0000: └ Completed
➤ YN0000: ┌ Fetch step
➤ YN0000: └ Completed
➤ YN0000: ┌ Link step
➤ YN0008: │ puppeteer@npm:19.6.2 must be rebuilt because its dependency tree changed
➤ YN0009: │ puppeteer@npm:19.6.2 couldn't be built successfully (exit code 1, logs can be found here: /tmp/xfs-2807b8a5/build.log)
➤ YN0000: └ Completed in 0s 487ms
➤ YN0000: Failed with errors in 0s 792ms
Error: building at STEP "RUN yarn --immutable": while running runtime: exit status 1
```
```sh 
[Warning] one or more build args were not consumed: [TARGETARCH TARGETOS TARGETPLATFORM]
```

## related
- [[kubernetes]]
- [[podman]]
- [[harbor]]
