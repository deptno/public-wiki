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

## cluster-role{-binding}
namespace 없이 전체 권한을 부여하고자 할때

## access api
+ https://kubernetes.io/docs/tasks/run-application/access-api-from-pod/

## 인증발급
+ [[kubernetes#authentication]]
- 인증이 발급 후에 롤에 권한이 추가되어도 추가 권한이 정상 동작

## link
- [[kubernetes]]
- [[service-account]]
- [[kubernetes-api]]
