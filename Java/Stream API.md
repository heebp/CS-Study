# Stream API

## 1. Stream API란?

Stream API는 Java 8에서 도입된 기능으로, 함수형 인터페이스(람다식)을 적용하여 컬렉션과 같은 저장요소를 반복적으로 처리할 수 있는 기능

> Stream API를 사용하기 위해서는 <U>Java 1.8 이상의 버전</U> 사용 <br>
> 해당 스트림 인터페이스는 <U>import java.util.stream</U>에 포함되어 있음

### **주요 특징**

- **선언적 프로그래밍** : Stream API를 사용하면 "무엇"을 할지 선언하는 방식으로 프로그래밍이 가능

- **함수형 프로그래밍** : 람다 표현식과 함께 사용하여 간결하고 표현력 있는 코드를 작성 가능

- **파이프라이닝** : 여러 작업을 연결하여 복잡한 데이터 처리 파이프라인 구성

- **내부 반복** : 컬렉션 내부에서 요소를 반복 처리하므로, 개발자가 직접 반복문을 작성할 필요가 없음

- **지연 연산** : 최종 연산이 호출되기 전까지는 중간 연산이 실행되지 않아 효율적인 처리가 가능

<br>

    📌 Stream API를 왜 알아야 할까?

    Stream API를 사용하기 전에는 데이터를 컬렉션(Collection)으로 처리해, 코딩이 길고 가독성이 불편함

    기존 Java 컬렉션 처리 방식 대비 코드 가독성 향상, 병렬 처리 가능, 유지보수성 향상 등의 이유로 Stream API를 사용

<br>

## 2. Stream API의 구조

![Stream](/Java/images/stream.png)

Stream은 크게 세 가지 주요 단계로 구성

1. <U>**Stream 생성**</U>

2. <U>**중간 연산(Intermediate Operations)**</U>

3. <U>**최종 연산(Terminal Operation)**</U>

### 2-1. Stream 생성

Stream은 다양한 방법으로 생성이 가능

- **컬렉션으로부터 생성** : `collection.stream()`

- **배열로부터 생성** : `Arrays.stream(array)`

- **숫자 범위로부터 생성** : `IntStream.range(1, 100)`

- **파일로부터 생성** : `Files.lines(path)`

- **직접 생성** : `Stream.of("a", "b", "c")`

#### Stream 생성 예시

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
Stream<String> stream = list.stream();
```

### 2-2. 중간 연산(Intermediate Operations)

Stream을 변환하거나 필터링하는 연산으로, 여러 개 연결이 가능하며, 결과로 새로운 Stream을 반환

> `filter(Predicate)`, `map(Function)`, `sorted()`, `distinct()` 등등

#### 중간 연산 예시

```java
Stream<String> filteredStream = stream.filter(s -> s.startsWith("a"));
```

### 2-3. 최종 연산(Terminal Operation)

Stream을 닫고 결과를 생성하는 연산

> `collect()`, `forEach()`, `reduce()`, `count()` 등

#### 최종 연산 예시

```java
List<String> result = filteredStream.collect(Collectors.toList());
```

<br>

## 3. Stream API의 장점

1. **가독성 향상** : 프로그램을 코딩할 때, 어떻게 할지를 하나하나 기술하는 '명령형' 방식으로 코딩하지 않고, 무엇을 하고싶다는 '선언적'으로 코딩이 가능

   #### 반복문으로 처리하는 기존방식

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

   // 이름이 "A"로 시작하고 길이가 4 이상인 이름을 찾아 정렬하여 출력
   System.out.println("Using traditional loop:");
   List<String> filteredAndSortedNames = new ArrayList<>();
   for (String name : names) {
       if (name.startsWith("A") && name.length() >= 4) {
           filteredAndSortedNames.add(name);
       }
   }
   Collections.sort(filteredAndSortedNames);
   for (String name : filteredAndSortedNames) {
       System.out.println(name);
   }
   ```

   #### Stream API를 사용하는 방식

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

   // 이름이 "A"로 시작하고 길이가 4 이상인 이름을 찾아 정렬하여 출력
   System.out.println("Using Stream API:");
   names.stream()
       .filter(name -> name.startsWith("A") && name.length() >= 4)
       .sorted()
       .forEach(System.out::println);
   ```

2. **유지보수성 향상** : Stream API로 인해 간결하고 명확한 코드로 데이터를 처리할 수 있어 가독성과 유지보수성이 향상

3. **병렬처리 지원** : Stream API에서 지원하는 병렬처리는 데이터의 흐름을 나누어서 멀티 스레드로 병렬로 처리하고 처리 후에 합치는 과정을 통해 대량의 데이터를 빠르고 쉽게 처리할 수 있음

   > 간단하게 `parallel()` 또는 `parallelStream()`이라는 연산을 추가하는 것만으로 병렬처리가 가능

<br>

## 4. Stream API 사용 예제

#### 이름 필터링 및 정렬

```java
List<String> names = Arrays.asList("John", "Jane", "Jack", "Doe");
List<String> result = names.stream()
    .filter(name -> name.startsWith("J"))
    .sorted()
    .collect(Collectors.toList());
System.out.println(result);
```

#### 숫자 합산

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream()
    .reduce(0, Integer::sum);
System.out.println(sum);
```

#### 병렬 스트림

```java
List<String> items = Arrays.asList("apple", "banana", "cherry");
items.parallelStream().forEach(System.out::println);
```

### Stream API 종류 참고자료

[Java] Stream API-1 이해하기: 용어 및 Stream 생성 <br>
https://adjh54.tistory.com/107

[Java] Stream API-2 이해하기: Stream 중간연산 <br>
https://adjh54.tistory.com/109

[Java] Stream API-3 이해하기: Stream 최종연산 <br>
https://adjh54.tistory.com/110

<br>

## 5. Stream API의 한계

1. **병렬 스트림 사용 주의** : 데이터 크기나 연산 복잡도에 따라 병렬 처리가 오히려 성능을 저하시킬 수 있음

   ```java
   // 작은 리스트에서는 오히려 느릴 수 있음
   List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
   int sum = numbers.parallelStream()
       .reduce(0, (a, b) -> a + b);
   ```

2. **스트림 재사용 불가** : Stream은 한 번 사용하면 닫히므로 재사용 시 `java.lang.IllegalStateException` 발생

   ```java
   // 예외 발생
   Stream<String> stream = list.stream();
   long count = stream.count();
   List<String> collected = stream.collect(Collectors.toList());
   ```

3. **디버깅 어려움** : 람다 표현식과 중첩된 연산으로 인해 디버깅이 어려움

   ```java
   // 디버깅용 출력이 많아져 실제 로직의 가독성 저하
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

   names.stream()
       .filter(name -> {
           System.out.println("Filtering: " + name);
           return name.startsWith("A");
       })
       .map(name -> {
           System.out.println("Mapping: " + name);
           return name.toUpperCase();
       })
       .sorted((a, b) -> {
           System.out.println("Sorting: " + a + " vs " + b);
           return a.compareTo(b);
       })
       .forEach(name -> System.out.println("Result: " + name));
   ```

<br>

### 참고 자료

https://www.jaenung.net/tree/2814

https://www.elancer.co.kr/blog/detail/255

https://mine-it-record.tistory.com/477

https://www.geeksforgeeks.org/stream-in-java/

#### Stream API 면접 질문 관련 참고 사이트

https://hgko-dev.tistory.com/465
