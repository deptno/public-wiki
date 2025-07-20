# postgresql

- create role
  user 와 함께 권한 생성
  + https://www.postgresql.org/docs/current/sql-createrole.html
## helm chart
+ https://github.com/bitnami/charts/tree/main/bitnami/postgresql/
- create database owner [ ROLE NAME ]

## 함수
- to_char - timestamp -> char
- date_trunc - timestamp 값을 유지하되 추출하는 방식으로 하여 원하는 값만 추출(시간단위 절삭등)


## trigger & event
function 을 정의한다 정의한다 정의에 사용할 수 있는 언어는 아래와 같다
- c 
- pl/plsql
- pl/python
node 에서 사용을 해야하는데 해당 언어를 지원하지 않으니 pl/plsql 을 통해서 event 를 발생키고 이를 listen 하는 방식의 구현이 가능하다
```mermaid
sequenceDiagram
  participant be as backend/node
  participant db as postgresql
  
  db ->> db: function F 정의
  note over db: `event 발송`
  db ->> db: trigger T 정의
  note over db: insert, update, delete 시 function F 실행
  be ->> db: LISTEN event
  be ->> db: add event listener on 'notification' 
  activate be
  db -->> db: insert evnet 발생
  db -->> db: trigger T 실행
  db -->> db: function F 실행
  db ->> be: send notification: event 'user_define_channel'
  deactivate be
  be ->> be: handle event
```

## error
- permission denied for database 
  + https://bobcares.com/blog/permission-denied-for-database-postgres/
- duplicate key value violates unique constraint 
  - ON CONFLICT (id) DO NOTHING
  - ON CONFLICT (id) SET UPDATE ...
### helm
#### upgrade 2023-04-18 
- 헬름 차트를 upgrade 했을때 에러가 출력되지 않았고 DEPLOYED 된 것으로 로그가 출력되어 그런가보다 했다. `values.yaml` 또한 적용되지 않았다.
```sh
$ helm history postgresql

REVISION        UPDATED                         STATUS          CHART                   APP VERSION     DESCRIPTION
3               Tue Apr 18 05:41:30 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
4               Tue Apr 18 05:51:28 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
5               Tue Apr 18 11:14:45 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
6               Tue Apr 18 12:11:03 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
7               Tue Apr 18 12:13:16 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
8               Tue Apr 18 12:14:51 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
9               Tue Apr 18 12:15:18 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
10              Tue Apr 18 12:15:25 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
11              Tue Apr 18 12:15:33 2023        superseded      postgresql-12.1.9       15.1.0          Upgrade complete
12              Tue Apr 18 12:28:38 2023        deployed        postgresql-12.1.9       15.1.0          Upgrade complete
```
  - 히스토리도 잘 출력되는 것을 확인할 수 있었으나 실제로는 적용이 되지 않았다
  - **중요** debugging 을 위해 `--debug --dry-run` 을 옵션을 추가하니 에러를 확인할 수 있었다.
  - '--set global.postgresql.auth.postgresPassword=$POSTGRES_PASSWORD' 를 추가하고 ENV 변수를 쉘 시작부분에 넣어서 주입하였으나 실패했다.
```sh
$ POSTGRES_PASSWORD=$(kubectl get secret --namespace "postgresql" postgresql -o jsonpath="{.data.postgres-password}" | base64 -d) helm upgrade -i postgresql chart/postgresql -n postgresql --debug --set global.postgresql.auth.postgresPassword=$POSTGRES_PASSWORD --dry-run

history.go:56: [debug] getting history for release postgresql
upgrade.go:142: [debug] preparing upgrade for postgresql
Error: UPGRADE FAILED: execution error at (postgresql/templates/secrets.yaml:17:24):
PASSWORDS ERROR: You must provide your current passwords when upgrading the release.
                 Note that even after reinstallation, old credentials may be needed as they may be kept in persistent volume claims.
                 Further information can be obtained at https://docs.bitnami.com/general/how-to/troubleshoot-helm-chart-issues/#credential-errors-while-upgrading-chart-releases

    'global.postgresql.auth.postgresPassword' must not be empty, please add '--set global.postgresql.auth.postgresPassword=$POSTGRES_PASSWORD' to the command. To get the current value:

        export POSTGRES_PASSWORD=$(kubectl get secret --namespace "postgresql" postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)

helm.go:84: [debug] execution error at (postgresql/templates/secrets.yaml:17:24):
PASSWORDS ERROR: You must provide your current passwords when upgrading the release.
                 Note that even after reinstallation, old credentials may be needed as they may be kept in persistent volume claims.
                 Further information can be obtained at https://docs.bitnami.com/general/how-to/troubleshoot-helm-chart-issues/#credential-errors-while-upgrading-chart-releases

    'global.postgresql.auth.postgresPassword' must not be empty, please add '--set global.postgresql.auth.postgresPassword=$POSTGRES_PASSWORD' to the command. To get the current value:

        export POSTGRES_PASSWORD=$(kubectl get secret --namespace "postgresql" postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)

UPGRADE FAILED
main.newUpgradeCmd.func2
        helm.sh/helm/v3/cmd/helm/upgrade.go:202
github.com/spf13/cobra.(*Command).execute
        github.com/spf13/cobra@v1.6.1/command.go:916
github.com/spf13/cobra.(*Command).ExecuteC
        github.com/spf13/cobra@v1.6.1/command.go:1044
github.com/spf13/cobra.(*Command).Execute
        github.com/spf13/cobra@v1.6.1/command.go:968
main.main
        helm.sh/helm/v3/cmd/helm/helm.go:83
runtime.main
        runtime/proc.go:250
runtime.goexit
        runtime/asm_arm64.s:1172
```
  - 분리된 라인으로 `export POSTGRES_PASSWORD=$(...)` 를 shell 자체에 주입을 하고 나서야 명령어가 성공하는 것을 확인할 수 있다.
```sh
$ export POSTGRES_PASSWORD=$(kubectl get secret --namespace "postgresql" postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)
$ helm upgrade -i postgresql chart/postgresql -n postgresql --debug --set global.postgresql.auth.postgresPassword=$POSTGRES_PASSWORD --dry-run

history.go:56: [debug] getting history for release postgresql
upgrade.go:142: [debug] preparing upgrade for postgresql
upgrade.go:150: [debug] performing update for postgresql
upgrade.go:313: [debug] dry run for postgresql
Release "postgresql" has been upgraded. Happy Helming!
NAME: postgresql
LAST DEPLOYED: Tue Apr 18 12:27:39 2023
```
  - 그리고 포트포워딩(localhost 를 위해)도 확인하자
### lock
```shell
SELECT pid, *
FROM pg_locks l
         JOIN pg_class t ON l.relation = t.oid AND t.relkind = 'r'
where relname ='[TABLE_NAME]'
```
pid 확인해서 컨테이너 접속후
```shell
kill [PID]
```

### ON CONFLICT DO UPDATE command cannot affect row a second time
```sh 
[2024-01-28 19:54:38] [21000] ERROR: ON CONFLICT DO UPDATE command cannot affect row a second time
[2024-01-28 19:54:38] Hint: Ensure that no rows proposed for insertion within the same command have duplicate constrained values.
```
- conflict 키 중복, 한 쿼리에 여러 아이템에대한 컨플릭트처리를 하는데 키 중복이 있었다.

## link
- [[kubernetes]]
- [[psql]]
- [[helm]]
- [[sql]]
- [[pgdump]]
- [[pgdumpall]]
