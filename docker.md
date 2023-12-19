# docker
컨테이너
```sh
brew install --cask docker 
```

## arg vs env
```Dockerfile
ARG JS

COPY ./$JS ./packages/$JS
CMD node $JS
```
둘다 $JS 로 표현되고 있지만 사용되는 구문에 따라 ARG 가 참조될지 ENV 가 참조될지 결정된다

- arg
  - build time 에 사용되므로 COPY 등 이미지가 만들어지는 타이밍에 사용된다
- env
  - runtime 에 사용된된다. `CMD` 가 실행되는 타이밍은 runtime 이므로 env가 참조된다
  
- **주의** 해야할 점은 사용을 해보니 `FROM` 절 전/후의 `ARG` 사용 용도가 다르다
  - `FROM` **전**의 `ARG` 는 `FROM` 절에서 사용 가능하다
  - `FROM` **전**의 `ARG` 는 `FROM` 후의 절에서 **사용 가능하지 않다**
  - `FROM` **후**의 `ARG` 는 `FROM` 이 후 절에서 사용 가능하다
  - `FROM` **후**의 `ARG` 는 `FROM` 절에서 **사용 가능하지 않다**

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

### `COPY packages ./packages` 컨텐츠 변경에도 캐시가 유지되는 문제
```Dockerfile
COPY packages ./packages
```
폴더안의 새로운 폴더가 생성되었음에도 불구하고 레이어가 캐시되는 문제가 있다.

### `failed to solve with frontend dockerfile.v0: failed to create LLB definition: no build stage in current context`
둘 중 하나 확인못함
- [X] ENV 는 FROM 전에 갈수 없다
  + https://qiita.com/kaizen_nagoya/items/1e8311a0ccda1413ef10
- [ ] ARG 를 잘못 쓴 것으로 보인다
- [ ] build 중 ctrl+c 로 취소하면서 문제가 생긴 것으로 보인데 빌드되다만 이미지를 삭제한다
  - Docker desktop -> 우상단의 debug 아이콘 -> Clean / Purge data

---

### `ERROR [internal] load metadata for [image]:[tag]`
항상 빌드되던 다커빌드시에 발생했는데 apt-get update 를 진행하는중에 발생했다. 좀 관련이 없어보이지만 결국 `docker system prune` 으로 해결되었다
```sh 
ERROR [internal] load metadata for [image]:[tag]
```
 다른 빌드를 할때는 용량 부족에 대한 에러가 발생했다
```sh 
failed to register layer: Error processing tar file(exit status 1): write /home/pptruser/.cache/puppeteer/chrome/linux-113.0.5672.63/chrome-linux64/ClearKeyCdm/_platform_specific/linux_x64/libclearkeycdm.so: no space left on device
```
no space 에러는 `docker system prune [-a]` 로 해결된다.
`FROM` 절에서 참조할 수 없는 이미지를 참조한 경우

### At least one invalid signature was encountered.
```sh 
docker system prune
```

### GPG error  
```sh
 [+] Building 3.4s (5/5) FINISHED
 => [internal] load build definition from Dockerfile.chrome                                                                                                                                                                        0.0s
 => => transferring dockerfile: 750B                                                                                                                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                                                                                                                  0.0s
 => => transferring context: 71B                                                                                                                                                                                                   0.0s
 => [internal] load metadata for docker.io/library/node:20-bullseye                                                                                                                                                                0.9s
 => CACHED [1/2] FROM docker.io/library/node:20-bullseye@sha256:c1b3e61fa0fde701bdb45032796bc4bb020a8c53f06cba5765584b620087fa03                                                                                                   0.0s
 => ERROR [2/2] RUN apt-get update && apt-get install gnupg wget -y &&   wget --quiet --output-document=- https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/google-archive.gpg &&     2.4s
------
 > [2/2] RUN apt-get update && apt-get install gnupg wget -y &&   wget --quiet --output-document=- https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/google-archive.gpg &&   sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' &&   apt-get update &&   apt-get install google-chrome-stable -y --no-install-recommends &&   rm -rf /var/lib/apt/lists/*:
#5 0.484 Get:1 http://deb.debian.org/debian bullseye InRelease [116 kB]
#5 0.589 Get:2 http://deb.debian.org/debian-security bullseye-security InRelease [48.4 kB]
#5 0.631 Get:3 http://deb.debian.org/debian bullseye-updates InRelease [44.1 kB]
#5 1.156 Err:1 http://deb.debian.org/debian bullseye InRelease
#5 1.156   At least one invalid signature was encountered.
#5 1.719 Err:2 http://deb.debian.org/debian-security bullseye-security InRelease
#5 1.719   At least one invalid signature was encountered.
#5 2.268 Err:3 http://deb.debian.org/debian bullseye-updates InRelease
#5 2.268   At least one invalid signature was encountered.
#5 2.306 Reading package lists...
#5 2.404 W: GPG error: http://deb.debian.org/debian bullseye InRelease: At least one invalid signature was encountered.
#5 2.404 E: The repository 'http://deb.debian.org/debian bullseye InRelease' is not signed.
#5 2.405 W: GPG error: http://deb.debian.org/debian-security bullseye-security InRelease: At least one invalid signature was encountered.
#5 2.405 E: The repository 'http://deb.debian.org/debian-security bullseye-security InRelease' is not signed.
#5 2.405 W: GPG error: http://deb.debian.org/debian bullseye-updates InRelease: At least one invalid signature was encountered.
#5 2.405 E: The repository 'http://deb.debian.org/debian bullseye-updates InRelease' is not signed.
------
executor failed running [/bin/sh -c apt-get update && apt-get install gnupg wget -y &&   wget --quiet --output-document=- https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/google-archive.gpg &&   sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' &&   apt-get update &&   apt-get install google-chrome-stable -y --no-install-recommends &&   rm -rf /var/lib/apt/lists/*]: exit code: 100

```
- GPG 에러 였지만 결국엔 disk space 랑 연결, 아래 명령어를 통해서 해결
```sh 
docker system prune -a
```

## link
- [[kubernetes]]
- [[podman]]
- [[harbor]]
- [[container]]
