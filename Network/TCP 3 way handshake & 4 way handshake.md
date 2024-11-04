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
- 서버는 클라이언트의 요청에 응답한 SYN_RCVD 상태

> [step 3] 클라이언트에서 서버의 연결 요청에 대한 ACK segment를 전송한다. 서버는 ACK segment를 받고 연결이 되어 ESTABLISHED 상태가 된다.

![3 way handshake 이미지](/Network/images/3-way-handshake.jpeg)

## 3. 3 way handshake TCP 헤더 필드
