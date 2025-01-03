ApplicationContext
=============

## ApplicationContext
> ApplicationContext란?
  - 스프링 컨테이너 중 하나이며 최상위 인터페이스인 BeanFactory를 상속받아 구현된 인터페이스이다.
  - BeanFactory가 가지고 있는 Bean 생성과 관계 설정뿐만 아니라 이벤트 처리와 같은 부가 기능도 담당한다.

> IOC
  - Inversion of Control(제어의 역전)이라는 뜻이다.
  - 제어의 역전은 기본적으로 개발자가 관여하는 것이 아님 프레임워크에서 제어한다는 의미이다.

> ApplicationContext의 Bean 처리 과정
  - @Configuration이라 적혀진 클래스 정보들을 저장한다.
  - @Bean이라 붙은 메소드 목록들로 빈을 생성한다.
  - 클라이언트에서 빈을 호출한다.
  - 클라이언트가 요청한 빈이 목록에 있는지 확인한다.
  - 설정한 클래스에 빈을 생성하도록 요청하고 생성된 빈을 반환 받는다.
  ![bean 이미지](/Spring/images/bean.png)

> 자바에서 ApplicationContext로 스프링 컨테이너 생성하기

  ![configuration 이미지](/Spring/images/configuration.png)
  - @Configuration 클래스를 만든다.

  ![applicationcontext 이미지](/Spring/images/applicationcontext.png)
  - 만들어둔 @Configuration 클래스를 등록하여 해당하는 객체를 가져온다.

### 참고 사이트
- https://velog.io/@max9106/Spring-ApplicationContext
- https://velog.io/@gehwan96/Spring-Boot-ApplicationContext%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90
- https://devscb.tistory.com/117
