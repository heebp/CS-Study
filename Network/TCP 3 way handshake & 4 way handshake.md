TCP 3 way handshake & 4 way handshake
=============

## 1. 3 way handshake란?
- TCP 장치에서 양쪽간의 정합성을 체크하는 하나의 논리적인 통신 방식
- 서로 데이터를 주고받을 때 준비가 되었다는 것을 보장해야만 통신을 할 수 있는데 3 way handshake가 그 방법 중 하나이다.
- 마치 처음만난 사람과 악수를 할 때 내가 먼저 악수를 요청하면 상대방이 해석해서 이에 대한 답변을 하는 것이 하나의 이치이다.

![악수 이미지](/Network/images/handshake.jpeg)

## 2. 3 way handshake 절차
> [step 1] 클라이언트가 서버에 연결에 대한 SYN segment를 요청하고 서버에서 클라이언트의 SYN segment를 수신한다.
- 클라이언트는 서버의 ACK segment를 기다리는 SYN-SENT 상태

> [step 2] 서버가 클라이언트에게 수신에 대한 ACK segment를 전송하고 연결에 대한 SYN segment를 요청한다. 클라이언트에서 ACK segment를 받으면 연결이 되어 ESTABLISHED 상태가 된다.
- 서버는 클라이언트의 요청에 응답한 SYN-RCVD 상태

> [step 3] 클라이언트에서 서버의 연결 요청에 대한 ACK segment를 전송한다. 서버는 ACK segment를 받고 연결이 되어 ESTABLISHED 상태가 된다.

![3 way handshake 이미지](/Network/images/3wayhandshake.jpeg)

## 3. 4 way handshake란?
- 3 way handshake가 연결을 위해 요청을 주고받는 방법이라면 4 way handshake는 세션을 종료하기 위한 방법 중 하나이다.
- 특이하게도 4 way handshake에서는 양쪽 모두에서 연결 종료를 요청할 수 있다.

## 4. 4 way handshake 절차
> [step 1] 클라이언트가 서버에 종료를 요청하는 FIN segment를 보낸다.
- 클라이언트는 FIN-WAIT-1 상태로 변경

> [step 2] 서버에서는 FIN Segment 요청을 받고 수신했다는 ACK segment를 전달한다.
- 서버는 자신의 통신이 끝날때까지 기다리는 상태인 CLOSE-WAIT로 변경
- 클라이언트가 ACK segment를 받으면 서버에서 남은 처리를 끝내고 FIN segment를 보내기를 기다리를 FIN-WAIT-2 상태로 변경
  
> [step 3] 서버에서 처리를 완료했다면 연결을 종료하겠다는 FIN segment를 클라이언트에 전달한다.
- 서버는 클라이언트로부터 ACK segment를 받을때까지 LAST-ACK 상태로 변경

> [step 4] 클라이언트는 FIN segment를 수신하고 ACK를 서버에 전달한다.
- 아직 서버로부터 받지 못한 데이터가 있을수 있으므로 TIME-WAIT 상태로 변경
- TIME-WAIT는 세션이 종료되고 FIN segment보다 늦게 도착하는 패킷이 유실될 것을 대비해 일정시간 기다리는걸 말한다.
- 서버는 ACK segment를 받고 CLOSED 상태로 변경되고 클라이언트도 일정 시간이 지나면 CLOSED 상태로 변경된다.

![4 way handshake 이미지](/Network/images/4wayhandshake.jpeg)

### 참고 자료
- https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake
- https://hojunking.tistory.com/106
- https://bangu4.tistory.com/74
- https://hojunking.tistory.com/107
