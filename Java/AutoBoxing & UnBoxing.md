# AutoBoxing이란?

원시 타입의 값을 해당하는 **Wrapper 클래스의 객체로 바꾸는 과정**을 의미합니다.

자바 컴파일러는 원시 타입이 아래 두 가지 경우에 해당될 때 **AutoBoxing**을 적용합니다:

1. **Wrapper 클래스 타입의 파라미터를 받는 메서드를 통과할 때**
2. **Wrapper 클래스 타입의 변수로 할당될 때**

---

## AutoBoxing의 예시

### 메서드 파라미터로 전달될 때

```java
public class Test {
    private int text;

    public Integer getText() {
        return text;
    }
}
```

위 코드에서 `getText` 메서드는 원시 타입인 `int` 값을 리턴하지만, 메서드의 리턴 타입은 `Integer` (Wrapper 클래스 타입)입니다.

### 실행 결과

해당 코드를 실행하면 **컴파일 오류가 발생하지 않습니다.**

#### 이유

자바 컴파일러가 자동으로 원시 타입인 `int` 값을 **Integer 값**으로 변환해주기 때문입니다.  
내부적으로는 아래와 같은 변환 과정이 숨어 있습니다:

```java
public class Test {
    private int text;

    public Integer getText() {
        return Integer.valueOf(text);
    }
}
```

`Integer.valueOf(text)` 메서드를 통해 **AutoBoxing**이 이루어지는 것입니다.

---

# UnBoxing이란?

**UnBoxing**이란 Wrapper 클래스 타입을 원시 타입으로 변환하는 과정을 의미합니다. 예: `Integer` → `int`

자바 컴파일러는 원시 타입이 아래 두 가지 경우에 해당될 때 **UnBoxing**을 적용합니다:

1. **Wrapper 클래스 타입이 원시 타입의 파라미터를 받는 메서드를 통과할 때**
2. **Wrapper 클래스 타입이 원시 타입의 변수로 할당될 때**

---

## UnBoxing의 예시

### Java 코드 예제

```java
// Java program to illustrate the concept
// of Autoboxing and Unboxing

import java.io.*;

class GFG {
    public static void main (String[] args) {
        // Creating an Integer Object with value 10
        Integer i = new Integer(10);

        // Unboxing the Object
        int i1 = i;

        System.out.println("Value of i: " + i);
        System.out.println("Value of i1: " + i1);

        // Autoboxing of char
        Character gfg = 'a';

        // Auto-unboxing of Character
        char ch = gfg;
        System.out.println("Value of ch: " + ch);
        System.out.println("Value of gfg: " + gfg);
    }
}
```

---

## Autoboxing / UnBoxing의 장점

1. **가독성 향상**: 개발자가 더 읽기 쉽고 간결한 코드를 작성할 수 있습니다.
2. **타입 변환의 편리함**: Wrapper 클래스 타입과 원시 타입을 상호 교환 가능하며, 명시적으로 타입 캐스팅을 수행하지 않아도 됩니다.

---

## 기본 타입과 박싱된 타입의 차이

1. **값과 식별성**:
   - 기본 타입은 값만 가집니다.
   - 박싱된 타입은 값과 식별성을 모두 가집니다. 따라서 값이 같아도 서로 다른 객체로 식별될 수 있습니다.

2. **NULL 허용 여부**:
   - 기본 타입은 `null`을 허용하지 않습니다.
   - 박싱된 타입은 `null`을 허용합니다.

3. **성능**:
   - 기본 타입이 박싱된 타입보다 시간과 메모리 사용 면에서 더 효율적입니다.

---

## Example 01: `==` 비교의 차이

아래는 두 숫자를 비교하는 프로그램입니다:

```java
public class BrokenComparator {
    public static void main(String[] args) {

        Comparator<Integer> naturalOrder =
            (i, j) -> (i < j) ? -1 : (i == j ? 0 : 1);

        Integer i1 = 42;
        Integer i2 = 42;

        int result1 = naturalOrder.compare(i1, i2);
        System.out.println(result1);

        int result2 = naturalOrder.compare(new Integer(42), new Integer(42));
        System.out.println(result2);
    }
}
```

### 결과

`==` 연산자는 **주소 비교**를 수행합니다. 따라서 `Integer(42)`와 `Integer(42)`는 서로 다른 객체이므로 `-1`을 리턴합니다.

#### 개선된 코드

```java
Comparator<Integer> naturalOrder = (preNum, laNum) -> {
    int i = preNum;
    int j = laNum;

    return (i < j) ? -1 : (i == j ? 0 : 1);
};
```

기본형으로 오토 박싱을 활용하면 정상적으로 작동합니다.

---

## Example 02: NPE 발생

```java
public class Unbelievable {
    static Integer i;

    public static void main(String[] args) {
        if (i == 42)
            System.out.println("what!!!");
    }
}
```

### 결과

`i == 42`와 같은 연산에서는 **박싱된 기본 타입의 박싱이 자동으로 풀립니다**.  
그러나 `i`가 `null`일 경우, **UnBoxing**이 발생하면서 `NullPointerException (NPE)`이 발생합니다.

---

## Example 03: 성능 문제

```java
Long sum = 0L;

for (long i = 0; i < Integer.MAX_VALUE; i++) {
    sum += i;
}
System.out.println(sum);
```

### 문제점

위 코드는 매우 느립니다. 이유는 `sum`이 박싱된 타입(`Long`)으로 선언되었기 때문에, 비교 연산 및 덧셈 과정에서 **박싱과 언박싱이 반복**되기 때문입니다.

#### 개선된 코드

```java
long sum = 0L;
int max = Integer.MAX_VALUE;

for (long i = 0; i < max; i++) {
    sum += i;
}
System.out.println(sum);
```

박싱된 기본 타입은 성능이 중요한 경우 피하고, 기본 타입을 사용하는 것이 좋습니다.

---

## 박싱된 기본 타입의 사용 사례

- **컬렉션의 원소, 키, 값으로 사용**: 컬렉션은 기본 타입을 담을 수 없으므로 Wrapper 클래스를 사용합니다.


