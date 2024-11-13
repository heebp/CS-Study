# 시스템 콜(System Call)

<details>
<summary>CPU 모드</summary>

1. 사용자 모드 (User Mode) <br>


    - 사용자 어플리케이션 코드 실행 <br>
    - 하드웨어(디스크, I/O 등) 접근 불가 <br>
      - 접근하기 위해서 시스템콜 사용 <br>

  <br>
  
  2. 커널 모드 (Kernel Mode) <br>
    - 운영체제(OS)가 CPU를 사용하는 모드

</details>

<br>

### 시스템 콜 <br>

: 어플리케이션이 운영체제에 있는 함수를 호출하는 것 <br>
: 커널 영역의 기능(하드웨어 접근)을 사용자 모드가 사용 가능하게 함

<br>

### 🤔 임의의 프로세스가 타 프로세스에 있는 함수를 호출 할 수 있을까?

ex) hwp 프로세스가 excel 프로세스에 있는 foo() 함수를 호출 가능? <br>
➡ **X**, 운영체제가 모든 프로세스가 각자의 영역에서만 활동하도록 제한

<br>

- **임의의 프로세스가 운영체제의 함수를 호출 할 수 있을까?**
  - 사용자 프로세스가 하드웨어에 접근해서 운영체제의 데이터를 저장, 읽기, 출력 등을 할 수 있는 함수 사용 <br>
    ➡ 이게 **시스템 콜**ㅋ

 <br>

> **Process는 User Mode에서 실행되다가 시스템 자원을 사용해야 할 때 **시스템콜**을 호출하여 Kernel Mode로 전환되어 작업을 수행 후 다시 User Mode로 전환**

![시스템콜도식화](/OperatingSystem/images/systemCall.png)

<br>

---

<br>

## 시스템콜 종류

### 1) 프로세스 제어 (Process Control)

- 끝내기(exit), 중지 (abort)
- 적재(load), 실행(execute)
- 프로세스 생성(create process)
- 프로세스 속성 획득과 속성 설정 (get process attribute and set process attribute)
- 시간 대기 (wait time)
- 사건 대기 (wait event)
- 사건을 알림 (signal event)
- 메모리 할당 및 해제

<br>

### 2) 파일 조작 (File Manipulation)

- 파일 생성 / 삭제 (create, delete)
- 열기 / 닫기 / 읽기 / 쓰기 (open, close, read, wirte)
- 위치 변경 (reposition)
- 파일 속성 획득 및 설정 (get file attribute, set file attribute)

<br>

### 3) 장치 관리 (Device Manipulation)

- 하드웨어의 제어와 상태 정보를 얻음 (ioctl)
- 장치를 요구(request device), 장치를 방출 (relese device)
- 읽기 (read), 쓰기(write), 위치 변경
- 장치 속성 획득 및 설정
- 장치의 논리적 부착 및 분리

<br>

### 4) 정보 유지 (Information Maintenance)

- getpid(), alarm(), sleep()
- 시간과 날짜의 설정과 획득 (time)
- 시스템 데이터의 설정과 획득 (date)
- 프로세스 파일, 장치 속성의 획득 및 설정

<br>

### 5) 통신 (Communication)

- pipe(), shm_open(), mmap()
- 통신 연결의 생성, 제거
- 메시지의 송신, 수신
- 상태 정보 전달
- 원격 장치의 부착 및 분리

<br>

### 6) 보호 (Protection)

- chmod()
- umask()
- chown()

<br>

---

[참고]
https://didu-story.tistory.com/311
