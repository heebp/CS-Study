# PCB와 Context Switching

## PCB(Process Control Block)란?

PCB(Process Control Block)는 프로세스를 표현하기 위한 자료구조로 Kernel 안에 존재

> Process Metadata들을 저장해 놓는 곳으로, 한 PCB 안에는 한 프로세스의 정보가 담김

- **Process Metadata**

1. **Process ID**: 프로세스 고유 식별 번호

2. **Process State(프로세스 상태)**: 프로세스의 현재 상태(준비, 실행, 대기 상태)를 기억

3. **Process Priority(스케줄링 정보)**: 프로세스 우선순위 등과 같은 스케줄링 관련 정보를 기억

4. **CPU Register**: 프로세스의 레지스터 상태를 저장하는 공간

5. **Owner(계정 정보)**: CPU 사용시간의 정보, 각종 스케줄러에 필요한 정보 기억

   이 외 **CPU Usage**, **Memory Usage**, **프로그램 카운터** 등

<br>

![PCB](/OperatingSystem/images/pcb.png)

**프로그램 실행** → **프로세스 생성** → **프로세스 주소 공간 생성** → **해당 프로세스의 메타 데이터들이 PCB에 저장**

#### PCB의 필요성

프로세스가 여러개 실행 중일 때 CPU에서는 프로세스의 상태에 따라 교체 작업이 진행

> Interrupt 발생 → 할당 받은 프로세스를 Waiting 상태로 변경 → 다른 프로세스를 Running 상태로 변경

이 때, 앞으로 다시 수행할 Block 상태의 프로세스에 대한 저장 값을 PCB에 저장

#### PCB의 관리방법

일반적으로 Linked List 방식으로 관리

> PCB List Head에 PCB들이 생성될 때마다 추가되며, 주솟값으로 연결이 이루어져 있는 연결 리스트 형태로, 삽입 삭제가 용이함

**즉, 프로세스가 생성되면 해당 PCB가 생성되고 프로세스 완료 시 제거**

이렇게 수행 중인 프로세스를 변경할 때, CPU의 레지스터 정보가 변경되는 것을 **Context Switching**이라고 함

<br>

## Context Switching이란?

수행 중인 Task(Process/Thread)가 변경될 때, CPU의 레지스터 정보가 변경되는 과정

> 즉, 이전 프로세스의 상태를 PCB에 보관하고, 다른 프로세스의 정보를 PCB에서 읽어와 CPU 레지스터에 적재하는 과정

보통 인터럽트가 발생하거나, 실행 중인 CPU 사용 허가시간을 모두 소모하거나, 입출력을 위해 대기해야 하는 경우에 발생

![Context Switching](/OperatingSystem/images/contextswitching.png)

#### Context Switching의 필요성

프로세스 수행 중 입출력 이벤트가 발생해서 해당 프로세스를 대기 상태로 전환시켰을 때 CPU를 그냥 놓게 놔두는 것보다 다른 프로세스를 수행시키는 것이 효율적

#### Context Switching의 Overhead

Context Switching에 걸린 시간과 메모리를 말함

프로세스 내부에서 System Call을 요청하거나 Interrupt가 외부에서 들어왔어도 운영체제에서 일을 마치고 다시 CPU로 넘어가게 될 때 발생

- **Context Switching 발생 시 소요되는 Cost**

1. Cache 초기화

2. Memory Mapping 초기화

3. 메모리의 접근을 위해 Kernel은 항상 실행

I/O 이벤트가 발생했을 때 디스크에 명령을 내렸는데 끝날 때까지 기다리면서 CPU가 가만히 있으면 CPU가 낭비 됨

> 즉, Overhead를 감수하면서 기존 프로세스를 새 프로세스로 변경하는 것이 이득

<br>

### 참고자료

https://github.com/Songwonseok/CS-Study/blob/main/OS/PCB%20Context%20Switching.md

https://nice-engineer.tistory.com/entry/PCBProcess-Control-Block-Context-Switching

https://jhkimmm.tistory.com/11
