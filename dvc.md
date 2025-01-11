# dvc
- data version
- [[git]] 과 함께 사용
- [[git-lfs]] 와 달리 git 과 버저닝을 분리

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
