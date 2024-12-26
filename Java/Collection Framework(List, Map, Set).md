# Java Collection Framework (JCF)

> Collection Framework
>
> - 자료구조(Data Structure) 종류의 형태들을 자바 클래스로 구현한 모음집
> - 데이터를 저장하는 자료구조와 데이터를 처리하는 알고리즘을 구조화하여 클래스로 구현해 놓은 것

### Collection Framework 장점

- 인터페이스와 다형성을 이용한 객체 지향적 설계를 통해 표준화 됨 <br>
  ➔ 사용법 익히기 편히라고 재사용성 높음
- 데이터 구조 및 알고리즘의 고성능 구현 제공 <br>
  ➔ 프로그램의 성능, 품질 향상 시킴
- 관련 없는 API간의 상호 운용성 제공 <br>
  ➔ 상위 인터페이스 타입으로 업캐스팅하여 사용
- 이미 구현되어있는 API를 사용함 <br>
  ➔ 새로운 API를 익히고 설계하는 시간 줄어듦
- 소프트웨어 재사용 촉진 <br>
  ➔ 만약 자바에서 지원하지 않는 새로운 자료구조가 필요하다면 컬렉션들을 재활용, 종합하여 새로운 알고리즘 만들 수 있음

<br>

> 컬렉션 프레임워크에 저장할 수 있는 데이터는 오직 **객체**(Object) 뿐임 (null도 가능) <br>
> 즉 int형이나 double형 같은 primtive 타입은 적재를 못함 <br>
> ⇨ wrapper 타입으로 변환하여 Integer 객체나 Double 객체로 박싱(Boxing)하여 저장해야 함

<br>
<br>

# JCF 종류

- 대부분의 컬렉션 클래스들은 List, Set, Map 중 하나를 구현하고 있음
- Vector, Stack, Hashtable과 같은 클래스들은 컬렉션 프레임워크가 만들어지기 전부터 존재하던 것들 (명명법 따르지X)
  - 호환을 위해 남겨진 것이므로 가급적 사용하지 않는 것이 좋음

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNL3ie%2FbtrXCH3ryaE%2FgYcDCWPH0YLIOkbEOQ5lK1%2Fimg.jpg)

<br>

## 1-1. Iterable 인터페이스

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd1MnrP%2FbtrXHBDeLNq%2FiQPGKl4K21riczxMRXIbAK%2Fimg.png)

- 컬렉션 인터페이스들의 가장 최상위 인터페이스
- Map은 iterable 인터페이스를 상속받지 않기 때문에 `iterator()`와 `spliterator()`는 Map 컬렉션에 구현되어 있지 않음 <br>
  ⇨ 스트림(Stream)을 사용하거나 간접적으로 키를 Collection으로 반환하여 루프문으로 순회하는 식으로 이용

#### 메서드

`forEach(Consumer<?super T> action)`

- 함수형 프로그래밍 스타일로 요소를 반복
- 컬렉션의 각 요소를 람다식으로 손쉽게 처리하고 싶을 때

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.forEach(name -> System.out.println(name));

// Alice
// Bob
// Charlie
```

<br>

`iterator()`

- 컬렉션의 요소를 하나씩 순차적으로 접근할 수 있는 Iterator 객체 반환
- 컬렉션의 요소를 읽거나 제거하는 데 유용

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Iterator<String> iterator = names.iterator();
while(iterator.hasNext()) {
  System.out.println(iterator.next());
}

// Alice
// Bob
// Charlie
```

<br>

`spliterator()`

- 병렬 처리를 위해 사용되는 Spliterator 객체 반환
- Spliterator는 컬렉션 데이터를 여러 조각으로 나누어 병렬 스트림(pipeline)처리를 가능하게 함
- 보통 Java Stream API와 함께 사용

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Spliterator<String> spliterator = names.spliterator();
spliterator.forEachRemaining(System.out::println);

// Alice
// Bob
// Charlie
```

<br>
<br>

## 1-2. Collection 인터페이스

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJJ6ql%2FbtrXHXy3XwJ%2FdCW7nFUvvlweuunJakhwqk%2Fimg.png)

- List, Set, Queue에 상속하는 **실질적인 최상위 컬렉션 타입**
- 업캐스팅으로 다양한 종류의 컬렉션 자료형을 받아 자료를 삽입, 삭제, 탐색 할 수 있음 ⇨ 다형성

#### 메서드

`add(Object o)`
`addAll(Collection c)`

`remove(Object o)`
`removeAll(Collection c)`

`contains(Object o)`
`containsAll(Collection c)`

<br>

`retainAll(Collection c)`

- 호출한 컬렉션과 주어진 컬렉션의 교집합만 유지

```java
List<String> names = new ArrayList<>(List.of("Alice", "Bob", "Charlie", "David"));
List<String> retainList = List.of("Alice", "David");

boolean result = names.retainAll(retainList);

System.out.println(result); // true
System.out.println(names);   // [Alice, David]

```

<br>

`toArray()`
`toArray(Object[] a)`

- 배열로 변환
- 인자로 배열을 주면, 주어진 배열 a의 타입과 크기에 맞게 컬렉션을 배열로 변환하고 반환

```java
List<String> names = List.of("Alice", "Bob", "Charlie");

Object[] namesArray = names.toArray();
System.out.println(java.util.Arrays.toString(namesArray));  // [Alice, Bob, Charlie]

String[] namesArray2 = names.toArray(new String[0]);
System.out.println(java.util.Arrays.toString(namesArray));  // [Alice, Bob, Charlie]
```

<br>

`size()`
`isEmpty()`
`clear()`
`equals()`

<br>

> ### 🤔 Collection 인터페이스 메서드에 데이터를 **get**하는 메서드가 없는 이유?
>
> - 각 컬렉션 자료형마다 구현하는 자료구조가 제각각이기 때문에 최상위 타입으로 조회하기 까다롭기 때문

<br>
<br>

## 2. List 인터페이스

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbdz44d%2FbtrXJrTKBPn%2Fy0xwEsdhVC8qczVpXUkAs0%2Fimg.png)

- 저장 순서 유지
- 요소 중복 허용
- 배열과 마찬가지로 index로 요소 접근
- 데이터의 양에 따라 **자료형의 크기가 동적으로 변함**
- **요소 사이에 빈공간 허용X** - 삽입/삭제마다 배열 이동

#### 메서드

`get(int idx)`

`add(int idx, Object element)`
`addAll(int idx, Collection c)`

`remove(int idx)`

`set(int idx, Object element)`

`ListIterator listIterator()`
`ListIterator listIterator(int idx)`

<br>

`indexOf(Object o)`
`lastIndexOf(Object o)`

- 지정된 객체 위치(idx) 반환
  - indexof: 순방향
  - lastIndexOf 역방향

<br>

`subList(int fromIdx, int toIdx)`

- 지정된 범위(from~to)에 있는 객체 반환

<br>

`sort(Comparator c)`

- 지정된 비교자(comparator)로 List 정렬

```java
List<String> names = new ArrayList<>(List.of("Alice", "Bob", "Charlie", "David"));

// 알파벳 순으로 정렬
names.sort(Comparator.naturalOrder());
System.out.println(names); // [Alice, Bob, Charlie, David]

// 길이 순으로 정렬
names.sort(Comparator.comparingInt(String::length));
System.out.println(names); // [Bob, Alice, David, Charlie]

```

<br>

### 2-1. ArrayList

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbrQPzA%2FbtrP4wHLNas%2Fr0bDZ9vOAPD1dLqjNOPj91%2Fimg.png)

- 배열을 이용하여 만든 리스트
- 데이터 저장 순서 유지, 중복 허용
- 데이터량에 따라 공간이 늘어나거나 줄어듦
- 단방향 포인터 구조로 자료에 대한 순차적인 접근에 강점이 있어 조회가 빠름
- **삽입/삭제 느림**
  - 단 순차적으로 삽입/삭제하는 경우는 빠름

### 2-2. LinkedList

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTMVWi%2FbtrXECWflnM%2FUsr8ErgkyVQpL3omFtYKP0%2Fimg.png)

- 노드(객체)를 연결하여 리스트처럼 만든 컬렉션 (배열 아님)
- 데이터의 중간 삽입/삭제가 빈번할 경우 빠른 성능 보장
- **임의의 요소에 대한 접근** 성능 좋지 않음
- 양방향 포인터 구조
- 스택, 큐, 트리 등의 자료구조의 근간

<br>
<br>

## 3. Set 인터페이스

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMYYn5%2FbtrXM4pSNKD%2FF6tiEd9P90IxehYtMrdB11%2Fimg.png)

- **데이터의 중복 허용하지 않고 순서를 유지하지 않는** 데이터의 집합 리스트
- 순서 자체가 없으므로 인덱스로 객체를 검색해서 가져오는 `get(index)` 메서드 존재X
- 중복 저장이 불가능 하므로 null 값도 하나만 저장 가능

#### 메서드

`add(E e)`

`contains(Object d)`

`iterator()`

`isEmpty()`

`size()`

`clear()`

`remove(Object o)`

<br>

### 3-1. HashSet

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FufHH0%2FbtrXD7bfLYU%2F6D01kkLHSBkm92KZBO86J0%2Fimg.png)

- 배열과 연결 노드를 결합한 자료구조 형태
- 가장 빠른 임의 검색 접근 속도
- 추가/삭제/검색/접근성이 모두 뛰어남
- 순서를 예측할 수 없음

<br>

### 3-2. LinkedHashSet

- 순서를 가지는 Set 자료
- 추가된 순서 또는 가장 최근에 접근한 순서대로 접근 가능
- 중복을 제거하는 동시에 **순서를 유지**하고 싶다면 HashSet대신 LinkedHashSet 사용

<br>

### 3-3. TreeSet

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnKgTk%2FbtrXJrMoazC%2FM7XzKTlgKd4NT9ndRKb77K%2Fimg.png)

- 이진 검색 트리(binary search tree) 자료구조 형태로 데이터 저장
- 중복 허용X, 순서 X
- **데이터를 정렬**하여 저장하고 있음
- 정렬, 검색, 범위 검색에 높은 성능

```java
Set<Integer> treeSet = new TreeSet<>();

treeSet.add(7);
treeSet.add(4);
treeSet.add(9);
treeSet.add(1);
treeSet.add(5);

treeSet.toString(); // [1, 4, 5, 7, 9] - 자료가 알아서 정렬됨
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKpe36%2FbtrXIyE98uP%2F6h8Ep8D7ICnsxD6RLaDBT1%2Fimg.png)

<br>
<br>

## 4. Map 인터페이스

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdu3dZy%2FbtrXIy6DQGw%2FInqkfUkz2xGYvo1GUsH8tk%2Fimg.png)

- 키(Key)와 값(Value)의 쌍으로 연관지어 이루어진 데이터 집합
- **값(Value)은 중복 저장 가능** / **키(Key)는 해당 Map에서 고유해야 함**
- 기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남음
- 저장 순서 유지X

#### 메서드

`put(Object key, Object value)`
`putAll(Map m)`

`clear()`

`remove(Object key)`

`containsKey(Object key)`
`containsValue(Object value)`

`get(Object key)`

`isEmpty()`

`size()`

<br>

`keySet()`

- Map에 저장된 모든 Key를 포함하는 Set 반환

`values()`

- Map에 저장된 모든 value를 포함하는 Collection 반환

<br>

> - 키(Key)는 중복을 허용하지 않기 때문에 Set 타입으로 반환
> - 값(Value)는 중복을 허용하기 때문에 Collection 타입으로 반환

<br>

`entrySet()`

- Map의 모든 key-value 쌍을 Map.Entry 타입의 객체로 저장한 Set을 반환

```java
Map<String, String> map = new HashMap<>();
map.put("A", "Apple");
map.put("B", "Banana");

Set<Map.Entry<String, String>> entries = map.entrySet();

for(Map.Entry<String, String> entry : entries) {
  System.out.println(entry.getKey() + " -> " + entry.getValue());
}

// A -> Apple
// B -> Banana
```

> #### `keySet()`과 차이
>
> - `keySet()`은 모든 키를 순회하며 가져오기 위해 추가적인 `get(key)` 호출 필요
> - `entrySet()`은 키와 값을 한번에 가져와 효율적
>
> ```java
> // keySet()
> for(String key : map.keySet()) {
>   System.out.println("Key: " + key + ", Value: " + map.get(key));
> }
>
> // entrySet()
> for (Map.Entry<String, Integer> entry : map.entrySet()) {
>     System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
> }
> ```

<br>

### 4-1. HashTable

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFwKRi%2FbtrXIQy0llI%2FKaFsgavJ31eDaJrXgW4040%2Fimg.png)

- 자바 초기 버전에 나온 레거시 클래스
- Key를 특정 해시 함수를 통해 해싱한 후 나온 결과를 배열의 인덱스로 사용해서 Value를 찾는 방식으로 동작
- HashMap보다는 느리지만 동기화가 기본 지원 됨
- 키와 값으로 null 허용X

<br>

### 4-2. HashMap

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkEInF%2FbtrXJ26Kjxx%2FvjvNojn62vkhnIOLn8831K%2Fimg.png)

- HashTable을 보완한 컬렉션
- 배열과 연결이 결합된 Hashing 형태로, 키-값을 묶어 하나의 데이터로 저장
- 중복 허용X, 순서 보장X
- 키와 값으로 null 허용
- 추가/삭제/검색/접근성 뛰어남
- 비동기로 작동하기 때문에 멀티스레드 환경에서 어울리지 않음 ]

<br>

### 4-3. LinkedHashMap

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcVTTrM%2FbtrXIktAmZS%2FaaUp8fuXbwCWk8RkbGycF0%2Fimg.png)

- HashMap을 상속하기 때문에 흡사하지만 Entry들이 연결리스트를 구성하여 **데이터의 순서 보장**
- 일반적으로 Map 자료구조는 순서를 가지지 않지만 LinkedHashMap은 들어온 순서대로 순서를 가짐

<br>
<br>
<br>

https://inpa.tistory.com/entry/JCF-%F0%9F%A7%B1-Collections-Framework-%EC%A2%85%EB%A5%98-%EC%B4%9D%EC%A0%95%EB%A6%AC
