# nc | NetCat
> peer to peer 로 접속하여 메시지를 전송할 수 있다
> 중요한 정보를 전송할때 흔적 남기지 않고 전송

```sh
nc -l [ip] [port] # 접속을 대기하고 연결되면 송수신 가능

nc [-v] [ip] [port] # 접속 후 채팅 처럼 데이터를 전송가능
echo 'something' | nc [ip] [port]
cat file | nc [ip] [port]
```

## link
- [[terminal]]
- [[nfs]]
- [[slack]]
- [[discord]]
