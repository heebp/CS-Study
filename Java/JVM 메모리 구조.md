# JVM (자바 가상 머신) 메모리 구조

> JVM(Java Virtual Machine) : 자바 돌리는 프로그램, 자바로 작성된 모든 프로그램은 JVM에서만 실행 될 수 있음
> ![](/Java/images/JVM.jpg) <br>
> 컴파일 된 .class 파일 처리를 거쳐 프로그램 실행

<br>

## JVM 동작 방식

- 자바 애플리케이션을 클래스 로더를 통해 읽어 자바 API와 함께 실행

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc7Bhxs%2FbtrOnrIhmOx%2FQwXlPzLKq5UXekkb11FWhk%2Fimg.png)

1. 자바 프로그램 실행 - JVM은 OS로부터 메모리 할당 받음
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트 코드(.class)로 컴파일
3. **Class Loader**는 동적 로딩을 통해 필요한 클래스(.class)들을 로딩 및 링크 - **Runtime Data Area**(실직적인 메모리를 할당 받아 관리하는 영역)에 배치
4. **Runtime Data Area**에 로딩 된 바이트 코드는 **Execution Engine**을 통해 명령어 단위로 읽어서 해석, 실행
5. **Execution Engine**에 의해 **Garbage Collector**의 작동과 **Thread** 동기화 이루어짐

<br>

## Runtime Data Area

- JVM의 메모리 영역
- 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적제하는 영역

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnwTr9%2FbtrIMCwazct%2FYXxiCWEvxFyU9GJ0XxTkhK%2Fimg.png)

<br>

| 영역                | 스레드 공유          |
| ------------------- | -------------------- |
| Method(Static)      | 모든 스레드에서 공유 |
| Heap                | 모든 스레드에서 공유 |
| Stack               | 각 스레드별로 생성   |
| PC Register         | 각 스레드별로 생성   |
| Native Method Stack | 각 스레드별로 생성   |

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbrvkNA%2FbtrIPYdYwMf%2FC5wCqtVNLvu642vOaGyCBK%2Fimg.png)

<br>

### 1) Method Area (= Class Area, Static Area)

- JVM이 시작될 때 생성되는 공간
- **바이트 코드**(.class)를 처음 메모리 공간에 올릴 때 **초기화되는 대상을 저장**하기 위한 메모리 공간
- JVM이 동작하고 클래스가 로드될 때 적재돼서 **프로그램이 종료될 때까지 저장**

<br>

모든 스레드가 공유하는 영역이라 다음과 같은 초기화 코드 정보들이 저장 됨

- Field Info : 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보
- Method Info : 메소드 이름, return 타입, 함수 매개변수, 접근 제어자 정보
- Type Info : Class인지 Interface인지 여부 저장, Type 속성, Super Class 이름
  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGHKtC%2FbtrIOvQXcpC%2FKCSnczBPq2rjJa3tzDUKek%2Fimg.png)

<br>

메서드 영역에는 **정적 필드**와 **클래스 구조**만 갖고 있다고 할 수 있음

<br>

> #### Runtime Constant Pool
>
> - 메서드 영역에 존재하는 별도의 관리 영역
> - 각 클래스/인터페이스 마다 별도의 constant pool 테이블 존재 - 클래스 생성할때 참조해야 할 정보들을 상수로 가지고 있는 영역
> - JVM은 Constant Pool을 통해 해당 메소드나 필드의 실제 메모리 상 주소를 찾아 참조
> - **상수 자료형을 저장하여 참조하고 중복을 맡는 역할**

<br>

### 2) Heap Area

- JVM이 관리하는 프로그램 상에서 데이터를 저장하기 위해 런타임 시 동적으로 할당하여 사용하는 영역
- **new 연산자로 생성되는 클래스와 인스턴스 변수, 배열 타입 등 Reference Type이 저장**됨
- Method Area 영역에 저장된 클래스만 생성되어 적재
- Garbage Collection 대상

<br>

힙 영역에 생성된 객체와 배열은 **Reference Type**으로써, JVM 스택 영역의 변수나 다른 객체의 필드에서 참조 됨 <br>
즉, 힙의 참조 주소는 **스택**이 갖고 있고 해당 객체를 통해서만 힙 영역에 있는 인스턴스를 핸들링 할 수 있음 <br>

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqEBMZ%2FbtrINtTDQbX%2FMJ2ODVUaLoVPvauKUBXsgK%2Fimg.png)

<br>

만일 참조하는 변수나 필드가 없다면 의미 없는 객체가 됨 ➔ JVM이 **Garbage Collector**를 실행시켜 제거 <br>

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdiVa4N%2FbtrIK82DPjx%2FY2VQwZAqs7LKoebT7kucI1%2Fimg.png)

<br>

### 3) Stack Area

- int, long, boolean 등 기본 자료형을 생성할 때 저장하는 공간
- **임시적으로 사용되는 변수나 정보들이 저장**
- LIFO

  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIWMvu%2FbtrINLlRy5q%2F47kyqs0O8ITVRXfXYvHS0K%2Fimg.png)

<br>

메서드 호출 시 마다 각각의 **스택 프레임**(그 메서드만을 위한 공간)이 생성됨 <br>
메서드 안에서 사용되는 값들을 저장, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시 저장 <br>
메서드 수행이 끝나면 프레임별로 삭제 <br>

> #### 데이터 타입에 따른 저장 방식
>
> - 기본(원시)타입 변수 : Stack 영역에 직접 값을 가짐
> - 참조타입 : Heap or Method 영역의 객체 주소를 가짐
>   ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FddIT0y%2FbtrILfIfvEw%2F7IkVFuVRKNYOSHhXq53bW1%2Fimg.png)

<br>

스택 영역은 각 스레드마다 하나씩 존재, 스레드가 시작될 때 할당 됨 <br>
프로세스가 메모리에 로드될 때 스택 사이즈가 고정되어 있어 런타임 시에 스택 사이즈를 바꿀 수 없음 <br>
고정된 크기의 JVM 스택에서 프로그램 실행 중 메모리 크기가 충분하지 않다면 **StackOverFlowError** 발생 <br>

<br>

### 4) PC Register

- **현재 수행중인 JVM 명령어 주소 저장**
- JVM 명령의 주소는 스레드가 어떤 부분을 무슨 명령으로 실행해야할 지에 대한 기록을 가지고 있음

#### 🤔 프로그램 실행?

- **CPU에서 명령어(Instruction)를 수행**하는 과정으로 이루어짐
- CPU 연산을 수행하는 동안 필요한 정보를 **레지스터**라고 하는 CPU 내의 기억장치를 이용
  <br>

ex) **A와 B라는 데이터와 피연산자 값인 Operand가 있고 이를 더하라는 연산 Instruction이 있을 때** <br>
A, B → 더하라는 연산 <br>
이때 A를 받고 B를 받는 동안 이 값을 CPU가 Register에 기억해둠

<br>
<br>

자바는 CPU에 직접 연산을 수행하도록 하는 것이 아닌 **현재 작업하는 내용을 CPU에게 연산으로 제공** <br>
**이를 위한 버퍼 공간**이 **PC Register** <br>
따라서 JVM은 스택에서 피연산값 Operand를 뽑아 별도의 메모리 공간인 PC Register에 저장하는 방식을 취함

![](/Java/images/JVM2.jpg)
![](/Java/images/JVM3.jpg)

<br>

### 5) Native Method Stack

- 자바 코드가 컴파일되어 생성되는 바이트 코드가 아닌 실제 실행 할 수 있는 **기계어로 작성된 프로그램을 실행**시키는 영역
- **자바 이외의 언어(C, C++, 어셈블리 등)로 작성된 네이티브 코드를 실행**하기 위한 공간
- 사용되는 메모리 영역으로는 일반적인 C 스택을 사용

> #### 실행 엔진 방식
>
> - JIT 컴파일러 : 바이트 코드 전체를 컴파일 하여 Native Code로 변경

<br>

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbdsmcb%2FbtrIToXCAgS%2FHCWqOKGfqJuMtjl8uKK0D0%2Fimg.png)

일반적으로 메서드를 실행하는 경우 JVM 스택에 쌓임<br>
해당 메서드 내부에 네이티브 방식을 사용하는 메서드가 있다면 해당 메서드는 네이티브 스택에 쌓임 <br>
네이티브 메서드 수행이 끝나면 다시 자바 스택으로 돌아와 작업 수행 <br>
➡ 그래서 네이티브 코드로 되어 있는 함수의 호출을 자바 프로그램 내에서도 직접 수행하고 결과를 받을 수 있는 것

<br>

#### JNI (Java Native Interface)

- 자바가 다른 언어로 만들어진 애플리케이션과 상호 작용할 수 있는 인터페이스를 제공하는 프로그램
- JNI가 사용되면 **네이티브 메서드 스택에 바이트 코드로 전환되어 저장**됨
- 실질적으로 제대로 동작하는 언어는 C/C++ 정도밖에 없음
