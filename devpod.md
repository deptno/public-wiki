# devpod
- devcontainer 환경을 로컬이 아닌 다른곳에서 띄울 수 있게한다
  - [[kubernetes]]
  - cloud
  - local
- 기본적으로 repository clone 혹은 local 디렉토리를 컨테이너로 복사하면서 파드가 시작된다
- 쿠버네티스 설정이 필요하다
  - devpod 에서 사용할 persistant-volume 이 필요하다
- devpod 는 cli 와 gui 클라이언트를 모두 지원한다
  - gui 를 먼저 설치하면 `Add CLI to Path` 를 통해 cli 설치가 가능하다
- 설정, gui 를 통해 설정함, 실제 저장은 `~/.devpod/config.yaml` 에 저장되는 것으로 보임
  - provider 설정을 통해 [[kubernetes]] namespace 를 설정한다
    - 미리 만들어둔 persistant-volume 이 존재해야한다
    - 미리 생성해둔 pv에 맞춰서 disk size 옵션을 설정한다 (eg. 10Gi)
    - kubernetes config `~/.kube/config`, 내 설정은 absolute path 로 설정되어 있으니 참고
    - kubernetes context 는 위의 config 파일을 읽고 설정하면 된다
    - 현재 내 옵션
      - create namespace `check`
      - dockerless disabled `false`
      - node selector `필요에 따른 개인값`
      - storage class `openebs-hostpath`, 개인값임
      - kubernetes pull secrets enabled `check`
        - 시크림 이름 설정하는 부분을 모르겠음
        - 실제로 `devpod` 네임스페이스에 secret 존재함

## link
- [[devcontainer]]
- [[kubernetes]]
