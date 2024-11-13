# OSI 7계층 (Open Systems Interconnetion 7 Layer)

**OSI 7계층** <br>
: 네트워크에서 통신이 일어나는 과정을 7단계로 나눈 것 <br>
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F995EFF355B74179035)

<br>

### 🤔 OSI 7계층을 나눈 이유

흐름을 한눈에 알아보기 쉽고, 특정한 곳에 이상이 생길 경우 다른 단계의 장비 및 소프트웨어를 건드리지 않고 이상이 생긴 단계만 고칠 수 있음

> **ex) PC방에서 게임을 하는 도중 연결이 끊겼을 경우**
>
> - 모든 PC에 문제 → 라우터 문제(3계층 네트워크) or 광랜 제공 회사의 회선 문제 (1계층 물리)
> - 한 PC만 문제 & 게임 소프트웨어 문제 → (7계층 어플리케이션)
> - 게임 소프트웨어에 문제X & 스위치에 문제 → (2계층 데이터링크)

<br>

---

<br>

## 1. 물리 계층 (Physical Layer)

- **전기적 신호가 나가는 물리적인 장비**
- 데이터만 전달, 데이터가 무엇인지 어떤 에러가 있는지 신경X
- 데이터를 전기적인 신호(0, 1)로 변환해서 주고받는 기능만 존재

> 전송 단위 : 비트(Bit) <br>
> 장비 : 케이블, 허브, 리피터, 모뎀 등 <br> > ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9927F73D5B73B8C735)

<br>

## 2. 데이터 링크 계층 (Data Link Layer)

- **물리적 매체를 통해 데이터 전달**
- 물리계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 **안전한 정보의 전달을 수행할 수 있도록** 도와주는 역할
- **통신에서의 오류 찾기 & 재전송**
- 맥 주소를 가지고 통신 - 각 기기들의 구분 가능
  - 맥 주소(MAC, Media Access Control Address) : 네트워크 인터페이스 카드의 고유 주소
- 포인트 투 포인트(Point to Point)간 신뢰성 있는 전송을 보장하기 위한 계층
  - CRC 기반의 오류 제어와 흐름 제어 필요

> 전송 단위 : 프레임(Frame) <br>
> 장비 : 브릿지, 스위치, 이더넷 등 <br> > ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9931734D5B73BA3D2F)

<br>

## 3. 네트워크 계층 (Network Layer)

- **경로(Route)와 주소(IP)를 정하고 패킷을 전달**
- **데이터를 목적지까지 가장 안전하고 빠르게 전달 (라우팅)** ➡ 최적의 경로 설정
- 네트워크를 논리적으로 구분하는 계층
  - 인터넷: IP주소(Internet Protocol Address)라는 논리적 주소 사용

> 전송 단위 : 패킷(Packet) <br>
> 장비 : 라우터

<br>

## 4. 전송 계층 (Transport Layer)

- **서비스를 구분하고 사용자들 간 신뢰성있는 데이터 전달**
- 오류검출 및 복구, 흐름제어, 중복검사 등 수행
- 데이터 전송을 위해 **Port 번호**사용
- 프로토콜:
  - TCP(Transmission Control Protocol, 신뢰성 있는 데이터 전송 목적) : 세그먼트 단위
  - UDP(User Datagram Protocol, 빠른 데이터 전송 목적) : 데이터그램 단위

> 전송 단위 : 세그먼트(Segment)

<br>

## 5. 세션 계층 (Session Layer)

- **두 컴퓨터 사이의 연결을 형성하고 유지 및 종료**
- 데이터가 통신하기 위한 논리적인 연결
- TCP/IP 세션을 만들고 없애는 역할
- 응용 프로세스가 통신을 관리하기 위한 방법 정의
- 데이터 전송 보안과 관련
- 4계층에서도 연결을 맺고 종료할 수 있기 때문에 어느 계층에서 통신이 끊어졌다 판단에는 한계가 있음

<br>

## 6. 표현 계층 (Presentation Layer)

- **전송하는 데이터의 표현 방식 결정** (데이터 변환, 압축, 암호화, 인코딩 등)
- 데이터 표현이 상이한 응용프로그램의 독립성 제공
- 해당 데이터가 텍스트인지, 그림인지, GIF인지, JPG 인지 등 구분

<br>

## 7. 응용 계층 (Application Layer)

- **응용 서비스를 수행하고 사용자 인터페이스를 제공**
- 사용자와 가장 가까운 계층, 최종 목적지
- **HTTP, FTP, SMTP**등의 프로토콜

<br>

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcMU43i%2FbtrQ0XE2yrq%2FaH7IRyCgbJOjw4fgs4TxW1%2Fimg.png)
