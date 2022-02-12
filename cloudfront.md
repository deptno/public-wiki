# cloudfront

## [[error]]
```
Access to XMLHttpRequest at 'https://XXX.com' from origin 'https://YYY.com' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```
해당 메시지에서 받은 응답코드는 `200` 이었다.
해당 처리는 [[s3]] 에서 [[cors]] 에 도메인 주입 후 cf invalidation 처리를 한다.

## related
- [[aws]]
- [[s3]]
