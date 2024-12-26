# Default Method
Default Method는 Java 8에서 추가된 기능으로, 인터페이스에 새로운 메서드를 추가하기 위한 방법이다.
이전에는 인터페이스에 메서드를 추가하면 모든 구현 클래스에서 해당 메서드를 구현했어야 했다.

---

## 기존 Interface 문제점
![image](https://github.com/user-attachments/assets/f366ca79-ddd2-48ae-bba7-fe7b62694691)

위 그림은 ClassA, ClassB, ClassC 총 3개의 클래스가 Interface A를 구현하고 있다.
이때, 요구사항이 추가되면서 Interface A에 특정 추상 메서드 method A를 추가해야된다면, 
인터페이스 원칙에 의해 ClassA, ClassB, ClassC에 모두 method A를 구현해야한다.

![image](https://github.com/user-attachments/assets/f90150cb-129c-4e4f-ac22-c503bb298c63)
다음과 같이 Interface A를 상속받는 무수히 많은 클래스가 있다면,
모든 Interface A의 구현체 클래스에 method A를 구현해줘야 하는 큰 문제점이 있다.

따라서 디폴트 메서드가 등장했으며, 이를 사용하면 구현 클래스에서 추가된 메서드를 일일이 오버라이드 하지 않아도 된다.

---

## default 메서드 구문
```
public interface MyInterface {
    // 추상 메서드
    void abstractMethod();

    // default 메서드
    default void defaultMethod() {
        // 구현 코드
    }
}
```
- default 키워드를 사용하여 메서드를 선언
- 중괄호 내부에 메서드의 구현 코드를 작성
- default 메서드는 기본적으로 public으로 선언되며, 필요에 따라 접근 제어자를 변경할 수도 있다.

---


## 사용 방법
**인터페이스 생성**
```
public interface MyInterface {
    // 추상 메서드
    void abstractMethod();

    // 기본 메서드
    default void defaultMethod() {
        // 기본 구현 코드
        System.out.println("This is a default method.");
    }
}
```

**인터페이스 구현**
```
public class MyClass implements MyInterface {
    @Override
    public void abstractMethod() {
        // 추상 메서드 구현 코드
        System.out.println("Abstract method implementation.");
    }
    
     public static void main(String[] args) {
        MyClass obj = new MyClass();
        obj.abstractMethod(); // 출력 : Abstract method implementation.
        obj.defaultMethod();  // 출력 : This is a default method.
    }
}
```
- 추상 메서드는 오버라이드하여 구현한다.
- 기본 메서드는 이미 구현되어 있기 때문에 호출하여 사용할 수 있다.

---

## 활용
1. 인터페이스의 기능 확장 (유연성, 확장성)
기존 인터페이스를 수정하지 않고도 새로운 기능을 추가할 수 있다.
유연성과 확장성을 높일 수 있다.

2. 라이브러리와 호환성 유지
Java 8 이후 라이브러리들은 기본 메서드를 활용하여 설계되었다.
기존 사용자 코드와 호환성을 유지하면서도 기능을 추가 및 수정할 수 있다.

3. 인터페이스의 다중 상속
여러 인터페이스에서 동일한 기본 메서드를 가질 수 있다.
충돌이 발생하면, 구현 클래스에서 해당 메서드를 오버라이드하여 해결할 수 있다.

4. 인터페이스의 선택적 구현
모든 메서드를 구현할 필요 없이, 필요한 메서드만 구현할 수 있다.
사용자에게 구현 부담을 줄여주고, 필요한 기능만 제공할 수 있다.

---



### 참고
- https://velog.io/@heoseungyeon/%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A9%94%EC%84%9C%EB%93%9CDefault-Method  
- https://cocococo.tistory.com/entry/Java-%EA%B8%B0%EB%B3%B8-%EB%A9%94%EC%84%9C%EB%93%9CDefault-Methods-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95  
- https://whybk.tistory.com/155
