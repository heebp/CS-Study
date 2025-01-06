# POJO, PSA, IoC/DI, AOP

### ✔ 스프링 주요 특징

- POJO 기반의 구성
- 의존성 주입(DI)을 통한 객체 간의 관계 구성
- AOP 지원
- 편리한 MVC 구조
- WAS의 종속적이지 않은 개발 환경

### 스프링 3대 핵심요소

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlWkuv%2Fbtq9MEmLHyN%2Fq6A6PnOdfE6L5AwdXjaOl0%2Fimg.png)

---

## POJO (Plain Old Java Object)

- 다른 클래스나 인터페이스를 상속받아 메서드가 추가된 클래스가 아닌 일반적으로 우리가 알고 있는 getter/setter와 같은 기본적인 기능만 가진 자바 객체
- 객체지향 원리에 충실한 객체
- 일반적인 자바 객체를 지칭

```java
public class User {
    private int id;

    public int getId() {
    	return id;
    }

    public void setId(int id) {
    	this.id = id;
    }
}
```

<br>

```java
// POJO 개념 사용
@Component  // Spring에 의해 관리되는 POJO 클래스
public class ExampleService {

    public void sayHello() {
        System.out.println("Hello from POJO!");
    }
}
```

```java
// POJO 개념 사용 X
public interface Service {
    void sayHello();
}

public class ExampleService implements Service {

    @Override
    public void sayHello() {
        System.out.println("Hello from non-POJO!");
    }
}
```

- 객체 간의 관계를 구성할 때 별도의 API를 사용하지 않음
- Java 코드를 이용해 객체를 구성하는 방식을 그대로 스프링에서 사용할 수 있음 <br>
  ➡ 개발할 때 특정 라이브러리나 컨테이너 기술에 종속적이지 않다는 것을 의미 <br>
- 가장 일반적인 형태로 코드를 작성하고 실행할 수 있기 때문에 생산성에서 유리함 <br>
- 코드에 대한 테스트 작업 역시 좀 더 유연하게 가능 <br>

<br>
<br>

---

## PSA (Portable Service Abstraction)

- 환경의 변화와 관계없이 일관된 방식의 기술로의 접근 환경을 제공하는 추상화 구조
- 다양한 서비스 제공 업체의 기술과 구현을 추상화하고 표준화된 방식으로 사용할 수 있는 인터페이스 제공
- POJO 원칙을 철저히 따른 Spring 기능
  - **Spring에서 동작할 수 있는 라이브러리들은 POJO 원칙을 지키게끔 PSA 형태의 추상화가 되어있음을 의미**

> #### PSA = 잘 만든 인터페이스
>
> - PSA가 잘 적용된 코드라면 나의 코드가 바뀌지 않고 다른 기술로 간편하게 바꿀 수 있도록 확장성이 좋고 기술에 특화되어 있지 않은 코드를 의미

### Spring에서 제공하는 PSA

- Spring Web MVC
  - **기존 코드를 거의 변경하지 않고** 웹 기술 스택을 간편하게 바꿀 수 있도록 해줌
  - HttpServlet을 상속받고 **doGet(), doPost()**를 구현하는 등의 작업을 하지 않아도 됨
  - 코드를 거의 그대로 둔 상태에서 톰캣이 아닌 다른 서버로 실행하는 것도 가능
- Spring Transaction
  - **기존 코드는 변경하지 않은 채로 트랜잭션을 실제로 처리하는 구현체를 사용 기술에 따라 바꿀 수 있음**
  - Low Level로 트랜잭션 처리를 하려면 명시적으로 setAutoCommit()과 commit(), rollback()을 호출해야 하지만 Spring이 제공하는 **@Transactional** 어노테이션을 사용하면 트랜잭션 처리 가능
- Spring Cache 등
  - **@Cacheable** 어노테이션을 붙여줌으로써 구현체를 크게 신경쓰지 않아도 필요에 따라 바꿔 쓸 수 있음

<br>
<br>

---

## IoC (Inversion of Control)

- 제어 반전 : 객체의 생성, 생명주기의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 것을 의미
- 컴포넌트 의존관계 설정(Component dependency resoulution), 설정(Configuration) 및 생명주기(LifeCycle)를 해결하기 위한 디자인 패턴

<br>

### IoC 컨테이너 (= 스크링 컨테이너)

> 컨테이너: 객체의 생명주기 관리, 생성된 인스턴스들에게 추가적인 기능을 제공

- IoC 개념을 구현하는 프레임워크의 컴포넌트
- 스프링 프레임워크에 있는 컨테이너
- 객체의 생성을 책임지고 의존성을 관리
- POJO의 생성, 초기화, 서비스, 소멸에 대한 권한 가짐 (개발자들이 직접 POJO를 생성할 수 있지만 컨테이너에게 맡김)
  - 개발자는 비즈니스 로직에 집중할 수 있음
- 객체 생성 코드가 없으므로 TDD(Test-Driven Development)가 용이

<br>

### IoC 분류

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fx4WUU%2Fbtq9gXndYek%2FCwB8xqIdfQh1ox2P9H6bV1%2Fimg.png)

- **DL** (Dependency Lookup, 의존성 조회)

  - 저장소에 저장되어 있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup하는 것
  - 의존성을 컨테이너에서 직접 가져오는 방식
  - **BeanFactory**, **ApplicationContext**에서 빈을 조회하는 방식

- **DI** (Dependency Ingection, 의존성 주입)
  - 각 클래스 간의 의존관계를 빈 설정(Bean Definition) 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것
  - 의존성을 컨테이너가 알아서 주입해주는 방식
  - **@Autowired**, **XML**설정 등
    - Setter Injection (수정자 주입)
    - Constructor Injection (생성자 주입)
    - Method Injection (필드 주입)

> #### 의존성 주입
>
> - 필요한 객체를 직접 생성하는 것이 아닌 외부로부터 객체를 받아 사용하는 것
> - Spring은 @Autowired 어노테이션을 이용해 의존성 주입
> - Test 코드가 용이해지고 코드의 재사용성을 높여줌
> - 객체 간의 의존성(종속성)을 줄이거나 없애줌
> - 객체 간의 결합도를 낮추면서 유연한 코드 작성 가능

<br>

     📌 DL 사용 시 컨테이너 종속이 증가하기 때문에 주로 DI를 사용함

<br>

### IoC 컨테이너 (= 스프링 컨테이너)의 종류

- 스프링 컨테이너가 관리하는 객체 : **빈(Bean)**
- 빈을 관리한다는 의미로 커네이너를 **빈 팩토리(BeanFactory)**라고 부름
- 보통 BeanFactory에 여러가지 컨테이너 기능을 추가하여 확장한 **ApplicationContext**를 이용

#### 1) BeanFactory

- Bean 등록, 생성, 조회, 반환 관리
- 팩토리 디자인 패턴을 구현한 것으로 빈을 생성하고 분배하는 책임을 지는 클래스
- Bean을 조회할 수 있는 getBean() 메소드가 정의되어 있음

#### 2) ApplicationContext

- Bean 등록, 생성, 조회, 반환 관리 기능은 BeanFactory와 같음
- 스프링의 각종 부가 기능을 추가로 제공
  - 국제화가 지원되는 텍스트 메세지 관리
  - 이미지 같은 파일 자원을 로드할 수 있는 포괄적인 방법 제공
  - 리스너로 등록된 빈에게 이벤트 발생 알려줌

<br>
<br>

---

## AOP (Aspect Oriented Programming)

- 관점 지향 프로그래밍 : OOP(Object Oriented Programming, 객체 지향 프로그래밍)을 돕는 보조적인 기술로 관심사의 분리(기능의 분리)의 문제를 해결하기 위해 만들어진 프로그래밍 패러다임
- **핵심 관심 사항**(Core Concern)과 **공통 관심 사항**(Cross-Cutting Concern)으로 분리시키고 각각 모듈화
  - Core Concern(핵심 기능) : 업무 로직을 포함하는 기능
  - Cross-Cutting Concern(부가 기능) : 핵심 기능을 도와주는 부가적인 기능
- OPP를 적용하여도 핵심 기능에서 부가 기능을 쉽게 분리된 모듈로 작성하기 어려운 문제점을 AOP가 해결
- **부가 기능을 Aspect로 정의**하여 핵심 기능에서 부가 기능을 분리함으로써 핵심 기능을 설계하고 구현할 때 객체 지향적인 가치를 지킬 수 있도록 도와주는 개념
- **필수적이지만 어쩔 수 없이 반복적으로 사용되는 코드들을 리팩토링할 수 있도록 해줌**
  - 이로인해 여러 곳에서 사용될만한 코드들이 한 곳에서 유지하고 관리할 수 있는 이점을 갖게 됨

<br>

### AOP 특징

- 프록시 패턴 기반
- 프록시가 호출을 가로챔 (Intercept)
- 메소드 JoinPoint만 지원
  - JoinPoint: 프로그램 실행 중에 Aspect(관점 코드)를 삽입할 수 있는 특정 지점
  - 즉 메소드가 호출될 때만 Aspect 적용

<br>

### 스프링 부트에 AOP 적용하는 법

- spring-boot-starter-aop dependency 적용
  - maven의 fom.xml / gradle의 build.gradle에 AOP 의존성 추가
- @EnableAspectJAutoProxy 어노테이션 추가
- 실제 AOP 로직 작성
  - AOP 클래스 설정 : @Aspect 에노테이션 추가
  - Spring 빈 등록 : @Component 어노테이션 추가

<br>
<br>
<br>
<br>

---

https://dev-coco.tistory.com/83 <br>
https://dev-coco.tistory.com/70
