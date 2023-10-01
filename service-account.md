# service-account

+ https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/

- [[pod]] 가 kubernetes api 를 사용하게 위해 갖는 인증
- user-account 와 service-account 는 구분된다
- `automountServiceAccountToken` 속성을 통해서 resource 에 자동 마운트 여부를 관리할 수 있다
  - 컨테이너의 `/var/run/secrets/kubernetes.io/serviceaccount/token` 에서 확인이 가능하다
  - 속성을 갖는 주체는 `ServiceAccount` 혹은 `Pod`(우선순위) 가 된다
- 모든 [[namespace]] 는 적어도 하나(`default`)의 [[service-account]] 를 가진다

```sh 
kubectl get serviceaccounts
```

## link 
- [[kubernetes]]
- [[service]]
- [[rbac]]
- [[kubernetes-api]]
