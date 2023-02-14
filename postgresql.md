# postgresql

- create role
  user 와 함께 권한 생성
  + https://www.postgresql.org/docs/current/sql-createrole.html
## helm chart
+ https://github.com/bitnami/charts/tree/main/bitnami/postgresql/

## error
- permission denied for database 
  + https://bobcares.com/blog/permission-denied-for-database-postgres/
- duplicate key value violates unique constraint 
  - ON CONFLICT (id) DO NOTHING
  - ON CONFLICT (id) SET UPDATE ...

## related
- [[kubernetes]]
