# pubsubhubbub
- [[websub]] 으로 정식명칭화 됨

## youtube :2025-02-10:
- [[youtube]] 채널에 대한 업데이트 이벤트를 받을 수 있는 구독 시스템에서도 사용됨
- `100`ms 정도의 딜레이 없이 구독하는 경우 쓰로틀 발생
- 액션시에 가끔 상태안좋다고 서버가 거부하기도하고 성공 이벤트 안오기도 함, backoff 등 처리 로직 필요
- `pubsubhubbub` 라이브러리 구현, 외부 접근이 가능한 url 필요, callback 이 이쪽으로 호출 되는 구조
  - `https://example.com/callback` 형태
- 인자
  - `hub` 주소는 `https://pubsubhubbub.appspot.com/` 를 사용
  - `topic` 은`https://www.youtube.com/xml/feeds/videos.xml?channel_id=${channelId}` 형태

### error
#### `pubsubhubbub` [[javascript]] 라이브러리
- `secret` 을 넣어서 서버를 구동한 경우 구독 해지는 되지만 feed 이벤트가 전달되지 않는 문제 발생

##### Buffer type log 
- [[node]]@20, `pubsubhubbub` `subscribe` 이벤트 파람에서 발생
- 코드
```ts
  logger.debug({
    file,
    feed: payload.feed,
    type: payload.feed.type,
  }, '문제파악')
```
- 결과, feed 만찍히고 type 은 안찍힘
  ```json
  {
    "level": "debug",
    "file": "/dist/handleFeed.mjs",
    "feed": {
      "type": "Buffer",
      "data": [
      ]
    },
    "msg": "문제파악"
  }
  ```
- `{ "type": "Buffer", "data": [...] }` 형태는 `Buffer` 타입을 로그 찍는 경우 발생, 사실상 `Buffer` 형태이기때문에 속성 접근이 안되는 것을 확인

## link
- [[youtube]]
- [[javascript]]
- [[node]]
