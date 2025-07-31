# zitadel

## installation
### [[helm]]
```sh
helm repo add zitadel https://charts.zitadel.com
helm repo update
```

- `9.0.0-rc-2` 기준으로 차트 에러가 너무 많고 구성도 좋지 않아서 사용해야할지 의문이 들 정도, 성공은 함
  + https://github.com/deptno/cluster-amd64/pull/6

## helm 설정 참고
- `envVarSecret` 을 생성해야함, 아래 기본 설정이 필요함, `env` 아님 주의
  ```sh
  ZITADEL_DATABASE_POSTGRES_DATABASE
  ZITADEL_DATABASE_POSTGRES_HOST
  ZITADEL_DATABASE_POSTGRES_USER_PASSWORD
  ZITADEL_DATABASE_POSTGRES_USER_USERNAME
  ZITADEL_DATABASE_POSTGRES_ADMIN_PASSWORD
  ZITADEL_DATABASE_POSTGRES_ADMIN_USERNAME
  ```
- `masterkey` 설정이 필요함 해당 속성을 가진 secret 이 사전 정의되고 차트에서 참조되어야함
- `login-client` secret 은 생성되나 [[helm]] 차트 제거시에도 삭제되지 않음 재설치시 이로인한 에러

## gg
- 문서가 부실, 관리자 페이지와 계정 설정이 복잡하고 클린설치등 차트 수준이 낮은거 같아 패스

## link
- [[authentication]]
