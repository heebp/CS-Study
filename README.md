# CS-Study

## ✅ 규칙
- 매주 목요일 3시
- 주제 2개 학습 ➔ 마크다운으로 정리 후 PR
- 10분 내외로 발표 후 팀원들에게 각각 하나씩 문제 출제
- [주차별 주제](https://github.com/S2gamzaS2/CS-Study/wiki)

## ✅ 주제

### 🔸 네트워크
- [OSI 7 계층](https://github.com/S2gamzaS2/CS-Study/blob/main/Network/OSI%207%EA%B3%84%EC%B8%B5.md)
- [TCP 3 way handshake & 4 way handshake](https://github.com/S2gamzaS2/CS-Study/blob/main/Network/TCP%203%20way%20handshake%20%26%204%20way%20handshake.md)
- [TCP/IP 흐름제어 & 혼잡제어](https://github.com/S2gamzaS2/CS-Study/blob/main/Network/TCP%20IP%20(%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%26%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4).md)
- [TCPvsUDP](https://github.com/S2gamzaS2/CS-Study/blob/main/Network/TCPvsUDP.md)
- [대칭키 & 공개키](https://github.com/S2gamzaS2/CS-Study/blob/main/Network/%EB%8C%80%EC%B9%AD%ED%82%A4%20%26%20%EA%B3%B5%EA%B0%9C%ED%82%A4.md)
- [HTTP & HTTPS](https://github.com/S2gamzaS2/CS-Study/blob/main/Network/HTTP%26HTTPS.md)
- 로드 밸런싱(Load Balancing)
- Blocking,Non-blocking & Synchronous,Asynchronous
- Blocking & Non-Blocking I/O

### 🔸 운영체제
- [운영체제와 컴퓨터](https://github.com/S2gamzaS2/CS-Study/blob/main/Network/%EB%8C%80%EC%B9%AD%ED%82%A4%20%26%20%EA%B3%B5%EA%B0%9C%ED%82%A4.md)
- [메모리 계층](https://github.com/S2gamzaS2/CS-Study/blob/main/OperatingSystem/%EB%A9%94%EB%AA%A8%EB%A6%AC%20%EA%B3%84%EC%B8%B5.md)
- [프로세스와 스레드](https://github.com/S2gamzaS2/CS-Study/blob/main/OperatingSystem/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%8A%A4%EB%A0%88%EB%93%9C.md)
- [CPU 스케줄링 알고리즘](https://github.com/S2gamzaS2/CS-Study/blob/main/OperatingSystem/CPU%20%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98.md)
- [인터럽트(Interrupt)](https://github.com/S2gamzaS2/CS-Study/blob/main/OperatingSystem/%EC%9D%B8%ED%84%B0%EB%9F%BD%ED%8A%B8(Interrupt).md)
- [시스템 콜(System Call)](https://github.com/S2gamzaS2/CS-Study/blob/main/OperatingSystem/%EC%8B%9C%EC%8A%A4%ED%85%9C%20%EC%BD%9C(System%20Call).md)
- PCB와 Context Switching
- 주소 공간과 가상 메모리(Address Space, Virtual Memory)
- 주소 변환(Address Translation)
- 세그먼테이션(Segmentation)
- 페이징(Paging)
- 가상메모리와 요구 페이징, 페이지 교체
- TLB(Translation Lookaside Buffers)
- Paging : Smaller Table
- 동기화(스핀락, 세마포어, 뮤텍스)
- 교착상태(DeadLock)
- 멀티프로세스, 스레드와 멀티스레딩


### 🔸 DB
- 키(Key)
- SQL - JOIN
- SQL Injection
- SQL vs NoSQL
- 이상(Anomaly)
- 정규화
- 인덱스(INDEX)
- 트랜잭션(Transaction)
- 트랜잭션 격리 수준(Transaction Isolation Level)
- 저장 프로시저(Stored PROCEDURE)
- 레디스(Redis)

### 🔸 JAVA
- 객체지향(상속, 다형성, 캡슐화 등)
- JVM 메모리 구조
- 컴파일 과정
- 다양한 GC(parellel, g1gc 등)
- JRE, JDK, JVM의 구분
- 자바 메모리관리(Xms, Xmx)
- Call by Value vs Call by Reference
- String Immutable(String constant pool, "a" vs new String("a"))
- Auto Boxing & UnBoxing
- Error vs Exception
- Checked vs UnChecked Exception
- 비동기처리 문법 비교
- Java 8의 특징
- Lambda(+ Functional Interface)
- Stream API
- Default Method
- Generic
- Reflection(Annotation)
- Collection Framework(List, Map, Set 등)
- HashMap
- Abstract Class vs Interface(default method)
- CountDownLatch & CyclicBarrier

### 🔸 Spring
- 웹 애플리케이션 이해
- 서블릿
- JSP
- MVC 패턴
- ApplicationContext
- PSA, IoC, AOP, POJO(각각에 대한 내용과 도입 이유)
- Bean(Scope)
- Filter, Interceptor
- @Autowired 주입 방법별 차이(Field, Setter, Constructor Injection)
- Spring vs Spring Boot
- Web MVC 요청 처리 과정(DispatcherServlet을 중심으로)
- @Controller vs @RestController
- ViewResolver
- @Valid 사용해서 DTO 검증
- Spring Security 개념, 인증 처리 과정, currentUser 정보

### 🔸 JPA
- JPA와 Hibernate
- 영속성 컨텍스트(캐시, 동일성보장, 변경감지, 트랜잭션 지연)
- Eager, Lazy Loading
- n+1 문제
- 다대다 해결 전략
- JPA의 캐시

<br>

## ✅ 커밋 규칙
- 커밋 메세지: [카테고리] 주제
- 예시
  ```
  git commit -m "[DB] SQL Injection"
  ```
