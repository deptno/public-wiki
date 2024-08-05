# awscli

## version@2
### s3
```sh
aws s3 cp [--profile=PROFILE] s3://bucket/path/ . --recursive --exclude "*" --include "*2022*"
```
pagination, 1000 개 이슈가 존재하는 듯 [[@todo]]

### ecr
#### [[error]]
##### 403 Forbidden
```sh 
ERROR: failed to solve: public.ecr.aws/lambda/nodejs:20: unexpected status from HEAD request to https://public.ecr.aws/v2/lambda/nodejs/manifests/20: 403 Forbidden
```
- 재 로그인한다, `us-east-1`
```sh 
docker logout public.ecr.aws
aws ecr get-login-password --profile [PROFILE] --region us-east-1 | docker login --username AWS --password-stdin [ACCOUNT_ID].dkr.ecr.us-east-1.amazonaws.com
```

##### no basic auth credentials
```sh
no basic auth credentials
```

## link
- [[aws]]
- [[terminal]]
- [[docker]]
