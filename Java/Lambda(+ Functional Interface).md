Lambda(+ Functional Interface)
=============

## 1. Lambda
> Lambda란?
  - 프로그래밍 언어에서 익명 함수를 지칭하는 말로 이름 그대로 함수 이름없이 내용물만 정의하는 개념

> Lambda의 주요 특징
  - 코드가 간결해진다.
  - 지연 연산을 지원하여 퍼포먼스 향상에 도움을 준다.(지연 연산은 해당 조건이 완료되어야 다음 연산이 진행되는 방식)
  - 모든 원소를 전부 순환하는 경우는 람다식이 조금 더 느리다.
  - 디버깅시 추적이 어렵다.
  - 람다식을 남용하면 가독성이 떨어진다.
  - 자바 8버전부터 지원한다.

> Lambda 예제
  - 0에서 9까지 숫자를 출력해보자
  ![for 이미지](/Java/images/for.png)
  ![lambda 이미지](/Java/images/lambda.png)

## 2. Functional Interface
> Functional Interface란?
  - 구현해야할 메소드가 하나만 정의되어있는 인터페이스
  ![functional interface1 이미지](/Java/images/functionalinterface1.png)
  ![functional interface2 이미지](/Java/images/functionalinterface2.png)

> 자바에서 제공하는 Functional Interface
  - Predicate<T>
    - T 타입을 받아 boolean 타입으로 리턴

  - Consumer<T>
    - T 타입을 받아 아무것도 리턴하지 않고 소비하는 일을 구현

  - Supplier<T>
    - 인자를 받지 않고 T타입을 리턴함

  - Function<T, R>
    - T 타입을 받아 R 타입으로 리턴
  
  - Comparator<T>
    - T 타입 인자 두개를 받아 int 타입으로 리턴

### 참고 사이트
- https://namu.wiki/w/%EB%9E%8C%EB%8B%A4%EC%8B%9D
- https://soobysu.tistory.com/102
- https://khj93.tistory.com/entry/JAVA-%EB%9E%8C%EB%8B%A4%EC%8B%9DRambda%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95
- https://bcp0109.tistory.com/313
