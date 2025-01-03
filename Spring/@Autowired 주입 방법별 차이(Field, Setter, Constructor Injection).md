@Autowired 주입 방법별 차이(Field, Setter, Constructor Injection)
=============

## 1. @Autowired
> @Autowired란?
  - 스프링 컨테이너에 등록된 빈에 대한 의존 관계가 필요할 때 사용하는 어노테이션
  - DI(의존성 주입)을 도와줌
  - @Autowired를 하는 방법은 Field, Setter, Constructor 총 3가지 방법이 존재

## 2. Field
> Field로 @Autowired 사용하기

  ![field 이미지](/Spring/images/field.png)
  - 필드에 @Autowired를 붙여서 의존성을 주입할 수 있다.
  - A 클래스와 B 클래스 사이에서 서로 참조할 때 오류가 발생할 수 있다.
  - final로 선언할 수 없어 객체 불변성을 보장하지 않는다.

## 3. Setter
> Setter로 @Autowired 사용하기

  ![setter 이미지](/Spring/images/setter.png)
  - setter 메서드를 생성하여 @Autowired로 의존성 주입을 할 수 있다.

## 4. Constructor
> Constructor로 @Autowired 사용하기

  ![constructor 이미지](/Spring/images/constructor.png)
  - 생성자를 사용하여 @Autowired로 의존성을 주입한다.
  - Field, Setter 방식은 빈을 먼저 생성하여 주입하려고 하지만 Constructor 방식은 주입하려는 빈을 먼저 찾는다.
  - 프로그램이 실행되기 전에 순환 참조 오류를 먼저 방지할 수 있다.
  - final 선언이 가능하여 불변성을 보장한다.
  - 생성자를 활용할 수 있어 테스트에 용이하다.

### 참고 사이트
- https://jaehoney.tistory.com/303
- https://m42-orion.tistory.com/100
- https://jackjeong.tistory.com/entry/Spring-%EC%83%9D%EC%84%B1%EC%9E%90-%EC%A3%BC%EC%9E%85-vs-%ED%95%84%EB%93%9C-%EC%A3%BC%EC%9E%85-Autowired
