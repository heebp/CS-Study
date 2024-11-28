# TCP/IP (흐름제어 / 혼잡제어)

## 들어가기 전

### TCP 통신이란?
- 네트워크 통신에서 신뢰적인 연결방식
- TCP는 기본적으로 `unreliable network`에서 `reliable network`를 보장할 수 있도록 하는 프로토콜
- TCP는 `network congestion avoidance algorithm`을 사용

### Reliable Network를 보장한다는 것은?
다음과 같은 4가지 문제점을 해결해야 함:
1. **손실**: 패킷이 손실될 수 있는 문제
2. **순서 바뀜**: 패킷의 순서가 바뀌는 문제
3. **Congestion**: 네트워크가 혼잡한 문제
4. **Overload**: 수신자가 과부하 상태가 되는 문제

---

## 흐름제어 / 혼잡제어란?

### 흐름제어 (Flow Control)
- 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법
- 수신측이 패킷을 지나치게 많이 받지 않도록 조절
- Receiver가 Sender에게 현재 자신의 상태를 Feedback하는 방식

### 혼잡제어 (Congestion Control)
- 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법
- 네트워크 혼잡을 방지하거나 해결하기 위한 기법

---

## 전송의 전체 과정
1. **Application Layer**: Sender Application Layer가 Socket에 데이터를 씀
2. **Transport Layer**: 데이터를 Segment에 감싸 Network Layer로 전달
3. **전송**:
   - Sender의 Send Buffer에 데이터 저장
   - Receiver는 Receive Buffer에 데이터 저장
4. **Application 처리**:
   - Application이 준비되면 Buffer에 있는 데이터를 읽기 시작
   - Flow Control의 핵심은 **Receiver Buffer가 넘치지 않도록 관리**하는 것
   - Receiver는 **RWND(Receive Window)**로 Receive Buffer의 남은 공간을 송신측에 알림

---

## 1. 흐름제어 (Flow Control)
- **문제**: 송신측이 수신측보다 데이터 처리 속도가 빠를 경우, 수신측의 제한된 저장 용량을 초과하면 데이터가 손실될 수 있음
- **해결책**:
  - 송신 측의 데이터 전송량을 수신 측에 따라 조절

### 해결 방법
#### Stop and Wait
![263B7D4E5715ECEB32](https://github.com/user-attachments/assets/bb4cf68e-4a60-4839-9b53-863ece8a6c88)



- 매번 전송한 패킷에 대해 확인 응답을 받아야만 다음 패킷을 전송하는 방식

#### Sliding Window (Go-Back-N ARQ)
- **설명**: 수신측에서 설정한 윈도우 크기만큼 송신측이 확인응답 없이 데이터를 전송
- **목적**: 전송은 되었으나 ACK를 받지 못한 바이트의 숫자를 파악하기 위해 사용하는 protocol

LastByteSent - LastByteAcked <= ReceivecWindowAdvertised

(마지막에 보내진 바이트 - 마지막에 확인된 바이트 <= 남아있는 공간) ==

(현재 공중에 떠있는 패킷 수 <= sliding window)

- **동작 방식**:
- 윈도우에 포함되는 모든 패킷을 전송
- 각 패킷의 전달이 확인되면 윈도우를 이동하여 다음 패킷 전송
![253F7E485715ED5F27](https://github.com/user-attachments/assets/4484d6eb-77c1-41bc-8dc7-de5093493f09)



- **Network**:
- TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있다. 호스트들은 실제 데이터를 보내기 전에 '3 way handshaking'을 통해 수신 호스트의 receive window size에 자신의 send window size를 맞추게 된다.

#### 세부구조
- **1. 송신 버퍼**:
  ![22532F485715EDF218](https://github.com/user-attachments/assets/c0893359-5f3d-4a40-92a9-7348ce567901)

  

- 200 이전: 이미 전송되고 ACK 수신 완료
- 200 ~ 202: 전송되었으나 ACK 받지 못한 상태
- 203 ~ 211: 아직 전송되지 않은 상태
- **2. 수신 윈도우**:
  ![25403A485715EE362B](https://github.com/user-attachments/assets/6ad61a8b-c6b7-4218-9daf-76adbab8dd4e)

  

- **3. 송신 윈도우**:
  ![2520244B5715EE6A14](https://github.com/user-attachments/assets/b470b933-0f71-4f44-a0ca-38b5a0bc70ea)

  

- 송신 윈도우보다 작거나 같은 크기로 지정해 흐름제어 가능
- **4. 송신 윈도우 이동**:
  ![227DC8505715EEBA0A](https://github.com/user-attachments/assets/007329b2-1c9f-4afb-b7ad-0899836f3024)

  

- Before: 203~204 전송 후 수신측은 ACK 203 전송
- After: 송신측은 205~209 전송 가능 상태로 윈도우 이동

- **5.Selective Repeat
- 수신측이 손실되지 않은 패킷만 ACK로 확인하여 재전송 최소화

---

## 2. 혼잡제어 (Congestion Control)
- **문제**: 네트워크 내 패킷 수가 과도하게 증가하여 혼잡 발생
- **혼잡**:
- 패킷 몰림으로 라우터가 처리 불가능한 상태
- 호스트가 재전송을 시도하면서 혼잡을 가중시킴
- **혼잡제어의 목적**:
- 송신측에서 데이터 전송 속도를 줄여 혼잡 방지

### 흐름제어와의 차이점
- **흐름제어**: 송신측과 수신측 간 전송 속도를 조정
- **혼잡제어**: 네트워크 전체 관점에서 전송 속도 조정

---

### 혼잡제어의 해결 방법

#### AIMD (Additive Increase / Multiplicative Decrease)
1. **동작 방식**:
 - 처음에 패킷을 하나씩 전송, 문제없이 도착하면 윈도우 크기를 1씩 증가
 - 패킷 손실 또는 일정 시간 초과 시 전송 속도를 절반으로 줄임
2. **특징**:
 - 공평한 방식으로 여러 호스트가 네트워크를 공유 가능
 - 단점: 초기 대역폭 사용이 낮고, 혼잡 발생 후 대역폭 조정

#### Slow Start (느린 시작)
1. **목적**: AIMD의 초기 속도 문제 해결
2. **동작 방식**:
 - 패킷을 하나씩 전송
 - 문제없이 도착하면 ACK 패킷마다 윈도우 크기를 1씩 증가
 - 한 주기 뒤 윈도우 크기가 지수적으로 증가
3. **특징**:
 - 혼잡 발생 후 윈도우 크기를 1로 줄임
 - 혼잡 발생 이전 윈도우 크기의 절반까지는 지수적 증가, 이후 1씩 증가

#### Fast Retransmit (빠른 재전송)
1. **동작 방식**:
 - 패킷 손실이 발생하면 수신측은 중복된 ACK 패킷을 송신측에 전달
 - 송신측은 중복 ACK 3개 수신 시 손실된 패킷 재전송
2. **특징**:
 - 혼잡을 감지하고 윈도우 크기를 줄임

#### Fast Recovery (빠른 회복)
1. **동작 방식**:
 - 혼잡 발생 시 윈도우 크기를 1로 줄이지 않고 절반으로 줄임
 - 이후 선형적으로 윈도우 크기를 증가
2. **특징**:
 - 혼잡 이후 AIMD 방식으로 동작

---
