# Interface vs Abstract Class

![](/Java/images/interface_abstract.jpg)

<Br>

### Interface

- 내부 모든 메서드는 `public abstract`로 정의 (default 메서드 제외)
- 내부 모든 필드는 `public static final` **상수**
- 클래스에 **다중 구현** 지원
- **인터페이스끼리는 다중 상속** 지원
- `static`, `default`, `private` 제어자를 붙여 클래스같이 \**구체적인 메서드*를 가질 수 있음
- 부모 자식 관계인 상속에 얽매이지 않고 공통 기능이 필요할 때마다 추상 메서드를 정의해 놓고 구현(implement)하는 식 <br>
  ➔ 추상 클래스보다 자유롭게 붙였다 뗏다 사용
- **인터페이스는 클래스와 별도로 구현 객체가 같은 동작을 한다는 것을 보장하기 위해 사용하는 것에 초첨**
- 다중 구현이 된다는 점을 이용해 내부 멤버가 없는 빈껍데기 인터페이스를 선언하여 **마커 인터페이스**로서 이용 가능

<Br>

### Abstract Class

- 하위 클래스들의 공통점들을 모아 추상화하여 만든 클래스
- 다중 상속 불가능, **단일 상속**만 허용
- 추상 메서드 외에 일반클래스같이 **일반적인 필드, 메서드, 생성자**를 가질 수 있음
- **추상화(추상메서드)를 하면서 중복되는 클래스 멤버들을 통합 및 확장** 가능
- 인터페이스와 다른점은 **클래스간의 연관 관계를 구축하는 것에 초점**
  - 부모 자식 간의 논리적으로 묶여있는 관계

<br>

## Interface / Abstract Class 사용처

- 인터페이스: `implements`라는 키워드처럼 인터페이스에 정의된 메서드를 각 클래스의 목적에 맞게 기능을 구현
- 추상 클래스: `extends` 키워드를 사용해 자신의 기능들을 하위클래스로 확장

<br>

### 추상클래스를 사용하는 경우

#### 1) 중복 멤버 통합

- 상속 받을 클래스들이 공통으로 가지는 메서드와 필드가 많을 때
- **상수 밖에 정의 못하는 인터페이스에서는 할 수 없는 기능**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FR0Gco%2FbtrOo7J901a%2FPCt90B1kl6MuQZeSHdhzqk%2Fimg.png)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFHzD3%2FbtrOqz7ICXN%2FBaz5Sm4kTsq2f4wKBvR9D0%2Fimg.png)

#### 2) 추상클래스의 다형성 이용 설계

- 클라이언트(ExamConsole)에서 자료형을 사용하기 전에 **미리 논리적인 클래스 상속 구조를 만들어 놓고 사용**이 결정되는 느낌
- 부모 추상클래스와 논리적으로 관련이 있는 확장된 자식클래스들을 다룸
  - 클라이언트와 추상화객체들은 **의미적으로 관계로 묶여있음**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3UWjW%2FbtrOpRHGdTQ%2FD3zLGMTpKia9D4I9wMuaEk%2Fimg.png)

```java
public class ExamConsole {
	Exam exam; // 상위 추상 클래스 타입으로 선언

    // 생성자 매개변수로 new NewlecExam() 혹은 new YBMExam() 생성자가 들어와 필드를 초기화
    // 생성자에서 다형성 적용
    ExamConsole(Exam e) {
    	this.exam = e; // 업캐스팅 초기화, 다양한 자식 클래스 객체로 초기화
    }

    void input() {}
    void print() {}
}
```

#### 3) 명확한 계층 구조 추상화

- 클래스끼리 **명확한 계층 구조**가 필요할 때
- 공통된 기능 구현이 필요하거나 공통으로 지켜야 할 규칙도 있을 때 상속을 통해 구조화하여 재정의(overriding)을 통해 구현

- SMS Sender 구현
  - 여러 통신사들이 다른 통신탑(`tower`)를 갖고 있어 접속할 때 각각 다른 구현 필요 : `eatablish Connection With Your Tower`
  - 공통으로 지켜야 할 규칙인 방해금지모드 : `check If Do not Disturb Mode`
    ⇨ 추상 클래스로 공통 분모를 추상화로 구현, 상속을 통해 여러 통신사 클래스를 확장하여 구현

```java
abstract class SMSSender {

    abstract public void establishConnectionWithYourTower();

    public void sendSMS() {
        establishConnectionWithYourTower();
        checkIfDoNotDisturbMode();
        // ...
        destroyConnectionWithYourTower();
    }

    abstract public void destroyConnectionWithYourTower();

    public void checkIfDoNotDisturbMode() {
        // 추상 클래스 안에서 구현
    }
}
```

```java
class SKT extends SMSSender {
    @Override
    public void establishConnectionWithYourTower() {
        // SKT 방식으로 커넥션 맺기
    }

    @Override
    public void destroyConnectionWithYourTower() {
        // SKT 방식으로 커넥션 종료
    }
}

class LG extends SMSSender {
    @Override
    public void establishConnectionWithYourTower() {
        // LG 방식으로 커넥션 맺기
    }

    @Override
    public void destroyConnectionWithYourTower() {
        // LG 방식으로 커넥션 종료
    }
}
```

<br>
<br>

### 인터페이스를 사용하는 경우

#### 1) 서로 논리적이지 않고 관련이 적은 클래스끼리 필요에 의해 형제 타입으로 묶을 때

- 인터페이스의 가장 큰 특징은 상속에 구애 받지 않은 상속(구현)이 가능하다는 것
- 자유로운 타입 묶음을 통해 추상화

<br>

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdPUIE4%2FbtrOqzy2iUL%2FayeIKxwEQzsGZLbW2AaRwk%2Fimg.png)

- `swimming()` 메서드를 각 자식 클래스에 추가해야 할 때
  -Creature 추상 클래스에 추상 메소드를 추가하면, 곧 이를 상속하는 모든 자손/자식 클래스에서 반드시 메소드를 구체화 해야한다는 규칙 때문에 실제로 수영을 못하는 호랑이(Tiger)와 앵무새(Parrot) 클래스에서도 메소드를 구현해야 하는 강제성이 생기게 됨
  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdHI2f5%2FbtrOqD99xBN%2FYS6EZ4pnNpeCukAiC4CLvK%2Fimg.png)

- 다니는 동작 메서드나, 말하는 동작 메서드를 각각 인터페이스마다 분리하여 선언하고 이를 각 자식 클래스에 자유롭게 상속시킴으로써 보다 구조화된 객체 지향 설계를 추구 할 수 있음
  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1n5Qc%2FbtrOqjiQJEw%2Fem2zX1kprIKPgJiUwfGH4K%2Fimg.png)

<br>

#### 2) 인터페이스 다형성 이용 설계

- 추상클래스는 클라이언트에서 자료형을 사용하기 전에 **미리 논리적인 클래스 상속 구조를 만들어 놓고 사용**이 결정
- 인터페이스는 반대로 먼저든 나중이든 **그때 그때 필요에 따라 구현해서 자유롭게 붙였다 뗏다**하는 느낌

<br>

- Filesaver 클래스는 구체적인 클래스 타입으로 통신하는 것이 아닌 인터페이스 라는 중개 타입을 이용하여 통신
  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4XX0y%2FbtrOq1QuzSo%2FH6bfPOQgqSvs2zvg1nme41%2Fimg.png)

- Exam, File, Rectangle 같은 서로 전혀 연관 관계가 없는 클래스들을 FileSaver 클래스에 전달해서 데이터를 파일로 저장하기 위해선, 인터페이스로 타입 통합하여 형제 관계를 구성하여 FileSaver 클래스의 인터페이스 객체 필드로 넘김
  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbewKyS%2FbtrOpRHVHQG%2FFlo8mKppiGTNB5hlQelH5k%2Fimg.png)

- 만일 분석 라이브러리를 사용한다고 했을때 Analyzer 클래스에서 통신으로 사용되는 Calculateable 인터페이스 타입 객체 필드에 Exam 클래스를 전달하기 위해 역시 다중 구현이 가능하다는 점을 이용해 Caculateable 인터페이스를 implements만 하면 됨
  ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvHIGo%2FbtrOpROKg0W%2Fk9nFnByv5QGBKbPoY2LdKK%2Fimg.png)

<br>
<br>
<br>

https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-vs-%EC%B6%94%EC%83%81%ED%81%B4%EB%9E%98%EC%8A%A4-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0
