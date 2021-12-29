# pinpoint

## versions
- @aws-amplify/analytics@1.4.3
  immediate 옵션은 flush config를 우회한다. 해당 코드는 아래와 같다.  
  ```typescript
  const BUFFER_SIZE = 1000;
  const FLUSH_SIZE = 100;
  const FLUSH_INTERVAL = 5 * 1000; // 5s
  const RESEND_LIMIT = 5;
  ``` 
