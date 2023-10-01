# rbac
> role based access control

## kubernetes
> role <- rolebinding -> subject

subject 에 role 을 binding 해서 권한을 주입한다

- binding
  - rolebinding
  - clusterrolebinding
- role
  - role - [[namespace]] 기반
  - clusterrole
- subject
  - user
  - [[service-account]]
  - group

## access api
+ https://kubernetes.io/docs/tasks/run-application/access-api-from-pod/

## link
- [[kubernetes]]
- [[service-account]]
- [[kubernetes-api]]
