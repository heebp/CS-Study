# Checked vs UnChecked Exception

![image](https://github.com/user-attachments/assets/875e97d9-3e0b-4d4c-a835-020773a46aa5)  

자바의 예외는 크게 **Error, Checked Exception, Unchekd Exception** 세가지로 분류된다.

## Error
에러는 시스템에 비정상적인 상황이 생길 경우 발생한다.
시스템 레벨에서 발생하는 오류이기 때문에 개발자가 예측하기 어렵고 처리할 수도 없다.
대표적인 에러로는 **OutOfMemory, StackOverFlow** 등이 있다.

## Exception
예외(Exception)는 개발자가 만든 어플리케이션에서 잘못된 상황이 발생했을 경우를 의미한다.
예외는 프로그램에 오류이기 때문에 복구가 가능한 수준이며 예외에 대한 예방 또한 가능하다.

예외는 **Checked Exception, Unchecked Exception**으로 나뉘고, 
둘의 구분은 **RuntimeException를 상속**하고 있는지와 **명시적인 예외 처리 강제 여부**이다.

### Checked Exception
- RuntimeException을 상속하지 않은 예외 클래스
- 명시적인 예외 처리를 강제하기 때문에 Checked Exception이라 한다.
- 반드시 try~catch로 예외를 잡거나, throw로 호출한 메소드에게 예외를 던져야 한다.

### Unchecked Exception
- RuntimeException을 상속하는 예외 클래스
- 명시적인 예외 처리를 강제하지 않기 때문에 Uncheked Exception이라고 한다.

| 구분       | Checked Exception                 | Unchecked Exception                   |
|------------|-----------------------------------|---------------------------------------|
| 확인 시점  | 컴파일(Compile) 시점              | 런타임(Runtime) 시점                  |
| 예외 처리  | 명시적 예외 처리 강제<br>(반드시 예외 처리해야함) | 명시적 예외 처리 강제하지 않음         |
| 트랜잭션 처리 | 예외 발생시 롤백하지 않음         | 예외 발생시 롤백해야함                |
| 종류       | IOException, FileNotFoundException 등 | NullPointerException, ClassCastException 등 |

### Checked Exception이 Rollback되지 않는 이유
기본적으로 Checked Exception은 복구가 가능하다는 메커니즘을 가지고 있다.
예를 들어, 특정 이미지 파일을 찾아서 전송해 주는 함수에서 이미지를 찾지 못 했을 경우 기본 이미지를 전송한다는 복구 전략을 가질 수 있다. 
결과적으로 복구가 가능하니 Rollback은 진행하지 않는다는 의미이다.

### 참고
https://hahahoho5915.tistory.com/67  
https://frankle97.tistory.com/23
