# telegram | 텔레그램

messager

## bot 만들기
+ https://somjang.tistory.com/entry/텔레그램-BotFather를-활용하여-텔레그램-봇-생성하고-chatid를-얻는-방법
1. BotFather 와 대화 시작
  - https://t.me/BotFather 접속해서 접근
  - telegram 에서 `BotFather` 검색해서 `start`
  - `api token` 획득
2. authorizing
  + https://core.telegram.org/api#bot-api
  + https://core.telegram.org/bots/api
  - curl "https://api.telegram.org/bot[token]/getMe"
3. 명령어 설정
  - 사용할 만한 method 를 찾는다
    - https://core.telegram.org/bots/api#setchattitle
    - https://core.telegram.org/bots/api#setchatdescription
    - https://core.telegram.org/bots/api#setmycommands
```sh 
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"commands":[{"command": "commanda", "description": "command a"}]' \
  https://api.telegram.org/bot[token]/setMyCommands
curl https://api.telegram.org/bot[token]/getMyCommands
```
    - https://core.telegram.org/bots/api#setchatmenubutton
4. 채팅
  - polling
     - telegram 에서 bot 을 찾아서 메시지 혹은 명령어를 입력한 후 아래 명령어로 확인한다
```sh 
curl https://api.telegram.org/bot[token]/getUpdates
```
  - webhook
    실제 서버는 반응성과 효율성을 위해서 webhook 으로 서비스 되어야한다
      - webhook 을 받을 수 있는 public domain 이 존재해야하며 https 가 지원되어야한다.
        - 사설 인증서를 사용한다면 해당 인증서를 등록해야한다
      - 주소로 해당이벤트가 오면 해당 post 메시지를 핸들링 하면된다.
5. 응답하기
```
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"chat_id": 1830393354, "text": "hello"' \
  "https://api.telegram.org/bot[token]/sendMessage"
```
