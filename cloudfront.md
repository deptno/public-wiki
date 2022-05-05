# cloudfront

## [[error]]
```
Access to XMLHttpRequest at 'https://XXX.com' from origin 'https://YYY.com' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```
해당 메시지에서 받은 응답코드는 `200` 이었다.
해당 처리는 [[s3]] 에서 [[cors]] 에 도메인 주입 후 cf invalidation 처리를 한다.

### 응답이 없는 경우
[[cors]] 관련해서 발생한 문제로 safari 에서는 응답코드가 존재하지 않고, 크롬에서는 403이 떨어지는등 일관적이지 않을 수 있다.
`OPTIONS` 을 받을 수 있도록 `GET, HEAD, OPTIONS` 이상의 옵션을 선택한다.

- https://aws.amazon.com/ko/premiumsupport/knowledge-center/no-access-control-allow-origin-error/#The_CloudFront_distribution.27s_cache_behavior_allows_the_OPTIONS_method_for_HTTP_requests

## related
- [[aws]]
- [[s3]]
- [[response-code]]
- [[cors]]
