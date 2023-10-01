# kubernetes-api

> /apis/{api-group}/{api-version}/namespaces/{namespace}/{resource-plural}

+ https://kubernetes.io/docs/tasks/run-application/access-api-from-pod/#without-using-a-proxy

- [[rbac]] 테스트하기 위해서는 [[container]] 내에서 아래를 실행한다 [[snippet]]
```sh 
# Point to the internal API server hostname
APISERVER=https://kubernetes.default.svc
SERVICEACCOUNT=/var/run/secrets/kubernetes.io/serviceaccount
NAMESPACE=$(cat ${SERVICEACCOUNT}/namespace)
TOKEN=$(cat ${SERVICEACCOUNT}/token)
CACERT=${SERVICEACCOUNT}/ca.crt
curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET ${APISERVER}/api
```

- keyword 로 검색하면서 헤더를 함께 볼 수 있는 [[snippet]]
```sh 
kubectl api-resources | head -n1; kubectl api-resources --verbs=list,get,create,update,patch,delete | unique | sort \
grep [SEARCH_KEYWORD]
```

## command
- `kubectl rollout restart -n $NAMESPACE deptno`
```sh 
curl --cacert ${CACERT} -X PATCH -H "Authorization: Bearer ${TOKEN}" -H "Content-Type: application/strategic-merge-patch+json" --data "{\"spec\":{\"template\":{\"metadata\":{\"annotations\":{\"kubectl.kubernetes.io/restartedAt\":\"$(date '+%Y-%m-%dT%H:%M:%S%:z')\"}}}}}" ${APISERVER}/apis/apps/v1/namespaces/${NAMESPACE}/deployments/deptno
```

## link
- [[kubernetes]]
- [[kubectl]]
- [[rbac]]
- [[service-account]]
