# redis

## 설치
- [[helm]] 을 통해서 기본값 설치 다음 에러와 함께 죽어있다. pvc 를 확인해보면 `PENDING` 상태
  ```sh 
  no persistent volumes available for this claim and no storage class is set
  ```
- `slave` 가 필요없어서 `replicaCount: 0`
- 개발 환경에서는 `persistence: false`
- 프로덕션 환경에서는 pvc, pv 생성 후 `existingClaim` 에 이름 명시

## link
- [[db]]
