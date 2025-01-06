# Bean(Scope)

## Spring Bean

![](https://gmlwjd9405.github.io/images/spring-framework/spring-bean.png)

    자바 어플리케이션은 어플리케이션 동작을 제공하는 객체들로 이루어져 있음
    이때, 객체들은 독립적으로 동작하는 것 보다 서로 상호작용하여 동작하는 경우가 많음
    이렇게 상호작용하는 객체를 "객체의 의존성"이라고 표현
    스프링에서는 스프링 컨테이너(IoC)에 객체들을 생성하면 객체끼리 의존성을 주입(DI; Dependency Injection)하는 역할을 함
    스프링 컨테이너에 등록한 객체들을 "빈"이라고 함

- Spring에서 POJO를 Bean이라고 부름
- Bean은 어플리케이션의 핵심을 이루는 객체이며 Spring IoC(스프링 컨테이너)에 의해 인스턴스화, 관리, 생성 됨
- 컨테이너에 공급하는 설정 메타 데이터(XML 파일)에 의해 생성됨
  - 컨테이너는 이 메타 데이터를 통해 Bean 생성, LifeCycle, Dependency등을 일 수 있음
- 어플리케이션의 객체가 지정되면 해당 객체는 `getBean()` 메서드를 통해 가져올 수 있음

### 주요 속성

- **class**(필수) : 정규화된 자바 클래스 이름
- id : Bean의 고유 식별자
- scope : 객체의 범위 (singleton, prototype)
- constructor-arg : 생성 시 생성자에 전달할 인수
- property : 생성 시 bean setter에 전달할 인수
- init method와 destroy method

```java
// XML based configuration file
<!-- A simple bean definition -->
<bean id="..." class="..."></bean>

<!-- A bean definition with scope-->
<bean id="..." class="..." scope="singleton"></bean>

<!-- A bean definition with property -->
<bean id="..." class="...">
	<property name="message" value="Hello World!"/>
</bean>

<!-- A bean definition with initialization method -->
<bean id="..." class="..." init-method="..."></bean>
```

<br>

## Spring Bean Scope

➡ **Bean이 관리되는 범위**

    스프링은 기본적으로 모든 Bean을 Singleton으로 생성하여 관리
    (기본 scope 전략이 Singleton)
    구체적으로는 어플리케이션 구동 시 JVM 안에서 스프링이 Bean마다 하나의 객체를 생성하는 것을 의미
    그래서 스프링을 통해 Bean을 제공받으면 언제나 주입받은 Bean은 동일한 객체라는 가정하에 개발을 함

![](https://gmlwjd9405.github.io/images/spring-framework/spring-bean-scope.png)

### 1) Singleton

- Spring 컨테이너에 한 번 생성됨
  - 컨테이너가 사라질 때 bean도 제거됨
  - 스프링을 통해 제공받은 bean은 언제나 동일한 객체라는 가정하에 개발
- 생성된 하나의 인스턴스는 single bean cache에 저장되고 해당 bean에 대한 요청과 참조가 있으면 캐시된 객체를 반환
  - 하나만 생성되기 때문에 동일한 것을 참조함
- 기본적으로 모든 bean은 scope이 명시적으로 지정되지 않으면 singleton
- 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 scope

> #### Singleton으로 적합한 객체
>
> - 상태가 없는 공유 객체
> - 읽기용으로만 상태를 가진 공유 객체
> - 공유가 필요한 상태를 지닌 공유 객체
> - 쓰기가 가능한 상태를 지니면서도 사용 빈도가 매우 높은 객체

![](https://velog.velcdn.com/images/may_yun/post/ea03b204-3247-4b2a-8361-12f9685e1e01/image.png)

```java
@Component
@Scope(value = "Singleton")
public class Singleton {
}
```

<br>

### 2) Prototype

- 모든 요청에 **새로운** 객체를 생성
  - 의존성 관계의 bean에 주입 될 때 새로운 객체가 생성되어 주입
  - 정상적인 방식으로 gc에 의해 bean 제거
  - 스프링 컨테이너에서 빈을 조회할 때 생성되고 초기화 메서드도 실행

![](https://velog.velcdn.com/images/may_yun/post/bdf64379-f772-4fea-902f-e23232397ae1/image.png)

```java
@Component
@Scope(value = "prototype")
public class ProtoType {
}
```

<br>
<br>
<br>

https://dev-coco.tistory.com/69 <br>
https://velog.io/@may_yun/Spring-Bean-Scope%EC%99%80-%EC%A2%85%EB%A5%98
