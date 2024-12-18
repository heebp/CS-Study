# Error vs Exception
Java에서는 프로그램 실행 중 발생할 수 있는 문제를 크게 오류(Error)와 예외(Exception)로 나눈다.

![image](https://github.com/user-attachments/assets/21d36053-f83a-48ad-9ab9-f4c8370e46b0)


**오류(Error)**  
오류(Error)는 시스템이 종료되어야 할 수준의 상황과 같이 수습할 수 없는 심각한 문제를 의미한다. 
개발자가 미리 예측하여 방지할 수 밖에 없다.

**예외(Exception)**  
개발자가 구현한 로직에서 발생한 실수나 사용자의 영향에 의해 발생한다. 
오류와 달리 개발자가 미리 예측하여 방지할 수 있기에 상황에 맞는 예외처리(Exception Handle)를 해야 한다.

예외가 발생하면 프로그램이 종료가 된다는것은 에러와 동일하지만 예외는 예외처리(Exception Handling)을 통해 프로그램을 종료 되지 않고 정상적으로 작동되게 만들어줄 수 있다.

자바에서 예외처리는 Try Catch문을 통해 해줄 수 있다.

---

## Throwable 
오류와 예외 모두 자바의 최상위 클래스인 Object를 상속받는다. 
그리고 그 사이에는 Throwable이라는 클래스와 상속관계가 있다.

이 클래스에 대한 공식문서를 읽어보면 이 클래스의 객체에 오류나 예외에 대한 메시지를 담는다는 이야기가 나온다. 
그리고 예외가 연결될 때(chained exception) 연결된 예외의 정보들을 기록하기도 한다고 한다.

이 Throwable 객체가 가진 정보와 할 수 있는 행위는 getMessage()와 printStackTrace()라는 메서드로 구현되어 있는데, 
이를 상속받은 Error와 Exception에서 두 메서드를 사용한다.

---

## Error 오류 
![image](https://github.com/user-attachments/assets/25b18d4b-ce0c-48aa-b84e-8ff53116c710)


**StackOverflowError**  
응용 프로그램이 너무 많이 반복되어 StackOverflow가 발생할 때 던져지는 오류이다


**OutOfMemoryError**  
JVM이 할당된 메모리의 부족으로 더 이상 객체를 할당할 수 없을 때 던져지는 오류이다.

- Garbage Collector에 의해 추가적인 메모리가 확보되지 못하는 상황일 때
- heap 사이즈 부족
- 너무 많은 class를 로드할 때
- 가용가능한 swap이 없을 때
- 큰 메모리의 native메서드가 호출될 때  
 이를 해결하기 위해, dump파일분석, jvm 옵션 수정 등이 있다.

앞서 말했듯 개발자가 미리 오류를 대처하기는 힘들다.
다만 StackOverflowError를 피하기 위해서 재귀를 사용할 때에 조심하거나 가시적인 loop를 사용하는 간접적인 예방이 가능하다.

또한 OutOfMemoryError를 피하기 위해 새는 메모리를 차단하거나 heap의 크기를 늘려주는 방법을 사용할 수도 있다.

---

## Exception 예외

![image](https://github.com/user-attachments/assets/493c8c97-2b24-4978-b771-419a74df49ed)

예외는 개발자가 구현한 로직에서 발생하며 다른 방식으로 처리가능한 것들로, JVM은 정상 동작한다.

---

## Exception의 2가지 종류
**1. Checked Exception**  
- 예외 처리가 필수적이며, 컴파일 시 체크된다.
  try-catch로 처리하거나 throws로 예외를 전달해야 한다.
- 주로 JVM 외부 리소스 사용 시 발생 (예: 파일 I/O, 네트워크 통신).

**2. Unchecked Exception**  
- RuntimeException의 하위 예외들
- 명시적인 예외 처리가 강제되지 않는다.
- 주로 프로그램의 논리적 오류로 인해 발생

Checked와 UncheckedException의 가장 명확한 구분 기준!  
**'꼭 처리를 해야 하는가'**

---

## 주요 예외 클래스 
- ArithmeticException: 0으로 나눌 때 발생
- ArrayIndexOutOfBoundsException: 배열 인덱스 범위 초과 시 발생
- ClassCastException: 잘못된 타입 캐스팅 시 발생
- NullPointerException: null 객체 참조 시 발생
- IllegalArgumentException: 잘못된 인자 전달 시 발생
- IOException: 입출력 작업 실패 시 발생
- NumberFormatException: 문자열을 숫자로 변환 실패 시 발생

---

## 예외 처리 방법
예외를 처리하는 방법은 크게 두 가지가 있다.
1. 직접 try-catch를 이용해서 예외에 대한 최종적인 책임을 지고 처리하는 방식
2. throws Exception을 이용해서 발생한 예외의 책임을 호출하는 쪽이 책임지도록 하는 방식
(주로 호출하는 쪽에 예외를 보고할 때 사용함) 다른 메서드의 일부분으로 동작하는 경우에 해당 방식 추천



### 참고
https://developmentrecord.tistory.com/entry/JAVA-%EC%97%90%EB%9F%ACError%EC%99%80-%EC%98%88%EC%99%B8Exception%EC%9D%98-%EC%B0%A8%EC%9D%B4
https://velog.io/@jipark09/Java-Error%EC%99%80-Exception-%EC%B0%A8%EC%9D%B4
