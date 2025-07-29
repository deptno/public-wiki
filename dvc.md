# dvc
- data version
- [[git]] 과 함께 사용
- [[git-lfs]] 와 달리 [[git]] 과 ~~버저닝을 분리~~
  - [[git-lfs]] 와 유사, 버저닝 자체는 [[git-commit]] 으로 진행됨
    - 이렇게 되면 [[git-lfs]] 와의 차별화가 줄어들어 가치는 줄어든다 생각됨
    - 다만, [[git]] 비대화를 피해 속도 유지, [[git-lfs]] 는 아닐 수 있음
    - 더 큰 파일 용량 처리
    - [ ] dvc 로 [[git]] 없이 데이터만 처리 가능, 확인 필요 [[todo]]
    - 다만 `*.dvc` 파일 history 를 가지고 [[git-checkout]] 을 진행할때 실제 파일 체크하는 점이 좀 다를 것

## error
### sftp 설정
+ [[sftp]] 를 remote 로 사용시에 에러
```sh 
dvc remote add -d project_name sftp://url:/path
Setting 'project_name' as a default remote.
ERROR: configuration error - config file error: Unsupported URL type sftp:// for dictionary value @ data['remote']['project_name']
```
  - 첫번째 에러는 `sftp` -> `ssh` 로 변경, [[sftp]] 도 [[ssh]] 를 사용
  - sftp 지원을 위해서는 아래 명령어를 통한 설치 필요
    ```sh 
    pip install 'dvc[ssh_gssapi]'
    ```
  - [[diary:2025-01-12]] 현재 머지가 덜되서 에러남 해결
    ```sh 
    pip install 'asyncssh==2.18.0'
    ```
    + https://github.com/fsspec/sshfs/issues/54

## link
- [[ai]]
- [[mlflow]]
- [[git]]
