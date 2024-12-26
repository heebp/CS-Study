# Generic

## 1. Generic이란?

Generic은 Java 5에서 도입된 기능으로, 데이터 타입을 일반화하여 클래스, 인터페이스, 메서드에서 사용할 수 있게 함

#### Generic 코드 예시

```java
// Generic을 사용하지 않은 코드
public class Box {
	private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}

// Generic을 사용한 코드
public class Box<T> {
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

### Generic을 사용하는 이유?

1. **타입 안정성 제공** : 컴파일 타임에 타입을 확인하여 잘못된 타입 사용을 방지

   ```java
   Box<String> box = new Box<>();
   box.set("Hello");
   String item = box.get(); // 타입 캐스팅 불필요
   ```

2. **코드 중복 제거 및 재사용성 향상** : Generic을 사용하면 다양한 타입에 대해 동일한 코드를 재사용 가능

   ```java
   // 타입별로 중복된 코드를 작성하지 않아도 됨됨
   List<Integer> intList = new ArrayList<>();
   List<String> stringList = new ArrayList<>();
   ```

3. **가독성 및 유지보수성 개선** : Generic을 사용해 의도를 명확히 하고, 코드 이해를 쉽게 함

<br>

## 2. Generic의 주요 문법 및 특징

### 2-1. 타입 파라미터

일반적으로 <>로 구분된 타입 매개변수는 <U>클래스 이름 뒤에 작성</U> <br>
타입 파라미터와 일반 클래스 또는 인터페이스 이름의 차이를 구분하기 위해서 정해진 규칙에 따라 타입 파라미터는 <U>단일 대문자를 사용</U>

- `<T>` : Type

- `<E>` : Element

- `<K>` : Key

- `<V>` : Value

- `<N>` : Number

```java
class MyClass<K, V> {
    private K first;
    private V second;

    void set(K first, V second) {
        this.first = first;
        this.second = second;
    }

    K getFirst() {
        return first;
    }

    V getSecond() {
        return second;
    }
 }

public class Main {
    public static void main(String[] args) {
        // Generic 클래스를 사용해 객체를 생성할 때, 구체적인 타입을 명시시
        MyClass<String, Integer> a = new MyClass<String, Integer>();

        a.set("hi",10);

        System.out.println("first data : " + a.getFirst());
        System.out.println("K Type : " + a.getFirst().getClass().getName());
        System.out.println("second data : " + a.getSecond());
        System.out.println("V Type : " + a.getSecond().getClass().getName());
    }
}

// 파라미터로 명시할 수 있는 것은 참조 타입(Reference Type)밖에 올 수 없음
```

### 2-2. Generic 메서드

클래스와 달리 반환타입 이전에 <> Generic 타입을 선언 <br>
<U>정적 메소드로 선언할 때</U> Generic 메서드 방식 사용 필요

```java
public static <T> void printArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}

// 객체를 생성할 때 <>안에 타입 파라미터를 지정
// T 타입은 메서드 호출 시에 호출하는 시점의 타입 인자로 결정
```

<br>

### 2-3. 바운디드 타입 파라미터

Generic 타입에서 타입 인자로 사용할 수 있는 타입을 제한하려는 경우 사용 <br>
특정 타입이나 상속 계층에 제한을 두어 타입 안정성을 강화

```java
<K extends T>  // T와 T의 자손 타입만 가능 (K는 들어오는 타입으로 지정 됨)
<K super T>	 // <T super [타입]> 은 존재하지않음

<? extends T>  // T와 T의 자손 타입만 가능
<? super T>  // T와 T의 부모(조상) 타입만 가능
<?>  // 모든 타입 가능 <? extends Object>랑 같은 의미
```

![Bounded Type Parameter](/Java/images/boundedtypeparameter.png)

- extends T : 상한 경계, 뒤에 오는 타입이 최상위 타입으로 한계가 정해짐

  - `<T extends Fruit>` : Fruit, Apple 타입만 올 수 있음

  - `<T extends Beef>` : Beef 타입만 올 수 있음

  - `<T extends Food>` : Food, Fruit, Meat, Beef 타입이 올 수 있음

  - 만약에 여러 개의 타입을 동시에 상속한 경우로 제한하고 싶으면 & 기호를 사용할 수 있음

- super T : 하한 경계, 뒤에 오는 타입이이 최하위 타입으로 한계가 정해짐

  - `<? super Fruit>`: Fruit, Food 타입만 올 수 있음

  - `<? super Beef>` : Beef, Meat, Food 타입만 올 수 있음

  - `<? super Food>` : Food 타입만 올 수 있음

<br>

### 2-4. 와일드 카드

<?>는 Generic 파라미터의 타입보다 사용하는 방법이 더 중요할 때 사용됨 <br>

**Unbounded Wildcard**라 부르며, 특정 타입에 종속되지 않고 어떠한 타입이든 올 수 있음을 의미

```java
public static <T> void printArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}
```

**`<? extends [타입]>`**

- 매개변수의 자료형을 특정 클래스를 상속받은 클래스로만 제한

**`<? supter [타입]>`**

- 매개변수의 자료형을 특정 클래스와 그 클래스의 상위클래스로만 제한

**`<T extends [타입]>`**

- 상속을 이용해서 T의 자료형을 제한

- 클래스 선언 시 사용하며, 인스턴스 생성 시 특정 클래스를 상속받은 클래스형만 인스턴스 내부에서 사용할 수 있도록 함

- 특정 인터페이스를 구현한 클래스만 사용하려는 경우에도 사용 가능

<br>

## 3. Generic과 관련된 제한사항

1. **Primitive 타입 사용 불가**

   - Generic은 깁본적으로 참조 타입(Reference Type)만 지원

   - Wrapper 클래스를 사용해야 됨 (예 : Integer 대신 int)

2. **런타임 타입 소거(Type Erasure)**

   - Generic 정보는 컴파일 타임에만 존재하며, 런타임에서는 소거됨

   - 런타임에는 `List<Integer>`와 `List<String>`이 동일하게 처리됨

3. **정적 멤버에 Generic 사용 불가**

   - 타입 파라미터는 클래스 인스턴스에 종속되므로, 정적 컨텍스트에서 사용할 수 없음

     ```java
     // 정적 멤버에서 Generics 사용 불가
     public class GenericClass<T> {
         private static T item;  // 컴파일 오류
     }
     ```

4. **배열 생성 제한**

   - Java는 타입 안전성을 보장하기 위해 Generic 타입의 배열 생성을 금지

     ```java
     // Generic 배열 생성 시 컴파일 오류
     List<String>[] array = new List<String>[10];  // 오류 발생
     ```

<br>

## 4. Generic의 장점

1. 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지 가능

2. 클래스 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없음

3. 비슷한 기능을 지원하는 경우, 코드 재사용성이 높아짐

<br>

### 참고 자료

https://eunsun-zizone-zzang.tistory.com/99

https://www.geeksforgeeks.org/generics-in-java/

https://developer-hm.tistory.com/41

https://seminzzang.tistory.com/163

https://st-lab.tistory.com/153
