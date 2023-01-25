# docker

컨테이너

```sh
brew install --cask docker 
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

## related
- [[kubernetes]]
- [[podman]]
