# iam

## AWS 에서의 API 인증
- API에 Authorization, Crendential 헤더가 포함된다.
- Authorization 에는 알고리즘이, Crendential 에는 access key + 알고리즘으로 서명된 sceret + 토큰(옵셔널) 이 포함된다.
- AWS SDK 는 이와같은 일을 자체적으로 처리해준다.

## IAM: identity access managment
### Root user
첫번째 IAM 유저를 생성한후 access key 자체를 비활성화 하는 것을 추천
MFA 도 활성화 할 것

### IAM Policy
- Principle: 대상
- Effect: Allow or Deny
- Action: 허용할 액션
- Resource: 타겟 리소스(s3 등)
- Condition: 조건을 걸어 대상을 좁힐수 있음

### Principle - 대상 타입
- aws access key
- iam user
- iam role - 서비스에 부여되는 정책으로 보면됨

sts(session token)을 사용하면 더 안전한 사용이 가능

RBAC -> ABAC

- Role Based Access Control
- Attribute Based Access Control

ABAC 을 하면 태그를 통해서 접근을 제어하는 IAM Policy 를 작성하는 것으로 편리하게 관리가 가능  
뭘 만들때마다 중복적으로 policy 생성을 하지 않아도 됨

### Identity-based policy vs Resource-based policy
- Identity-based policy 은 대상에 연결된다.
- Resource-based policy 은 타겟 리소스에 연결된다.
- policy에서 principle 에 **빠지면** resource-based policy 로 간주된다.
- 동일 aws account 내에서는 두 policy 의 **합집합**으로 퍼미션이 결정된다.
- 크로스 aws account 에서는 두 policy 의 **교집합**으로 퍼미션이 결정된다.

## reference
- [AWS IAM과 친해지기 - 김태수](https://www.youtube.com/watch?v=c70glL9Znzs)
