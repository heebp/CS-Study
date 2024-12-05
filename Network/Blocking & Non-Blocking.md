# Blocking & Non-Blocking I/O

## I/O란?

I/O는 입력(Input)과 출력(Output)의 약자로 운영체제에서 I/O는 일반적으로 컴퓨터 시스템이 외부 입출력 장치들과 데이터를 주고 받는 것을 의미

> I/O 작업은 User레벨에서 직접 수행할 수 없고, 실제 I/O 작업을 수행하는 위치는 Kernel(운영체제)에서만 가능

<br>

## Blocking I/O

Blocking I/O는 I/O 작업이 진행되는 동안 User Process가 자신의 작업을 중단한 채, I/O가 끝날때까지 대기하는 방식

> 가장 기본적인 I/O 모델로, Linux에서의 모든 소켓 통신은 기본 Blocking으로 동작

![Blocking I/O](/Network/images/blockingio.png)

- **Blocking I/O의 동작 과정**

1. Process(Thread)가 Kernel에게 I/O를 요청하는 함수를 호출 (**제어권을 넘김**)

2. Process(Thread)는 작업 결과를 반환 받을 때까지 대기 (**제어권을 넘겨줬으므로 대기, 기존 작업 제어 불가**)

3. Kernel이 작업을 완료하면 작업 결과를 반환 받음 (**제어권을 다시 넘겨받음**)

<br>

이 경우, 말 그대로 Block이 되고, 애플리케이션에서 다른 작업을 수행하지 못하고 대기하게 되므로 Resource 낭비가 발생함

> 예를 들어, 카카오톡이 사용자가 메세지를 전송할 때까지 대기하고 있는거라고 생각하면 됨 (이 외에 다양한 기능이 많음에도 불구하고)
