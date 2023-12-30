# meilisearch
- [[rust]] 기반의 [[elastic-search]] 대체제

## 설치
### [[helm]]
```sh 
helm pull meilisearch/meilisearch --untar
# 버전 관리를 위해
mv meilisearch meilisearch@x.y.z

helm upgrade -n [NAMESPACE] --install meilisearch meilisearch@x.y.z
```

#### 설정
- `fullnameOverride` 이름 설정
- `environment.MEILI_ENV` `production` 을 하면 자동으로 secret 생성됨


## 운영

### 인증
- master key 가 사용되면 public api 가 모두 닫힌다.
- master key 를 통해서 권한을 가질 수 있고 key 관리가 가능하다
- `/keys` 를 통해서 키 관리가 가능하고 기본적으로 API key와 관리할 수 있는 key가 제공된다
- API key의 경우 frontend 에서 호출할때 포함시켜서 검색을 하는데 사용


## link
- [[rust]]
- [[deptno-dev]]
- [[helm]]
