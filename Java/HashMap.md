HashMap
=============

## HashMap
> HashMap이란?
  - Key와 Value의 쌍 구조로 저장되는 데이터 자료구조
  - 해시함수를 사용해 Key값의 저장 위치를 결정함
  - 데이터의 추가, 삭제, 검색이 빠름
  - Key는 중복될 수 없고 Value는 중복될 수 있음

> HashMap 생성 방법
  ![hashmap 이미지](/Java/images/hashmap.png)
  - 첫번째 인자는 데이터 크기, 두번째 인자는 데이터 저장 공간을 추가로 확보해야되는 시기를 의미한다.
  - 기본적으로 16의 데이터 저장 공간과 0.75일 때 저장 공간을 확보한다.

> HashMap 제공 메소드
  - 데이터 삽입
    - V put(K key, V value) : 데이터를 key와 value 쌍으로 저장한다.
    - V putIfAbsent(K key, V value) : 기존의 데이터에 key가 없으면 key와 value를 저장한다.

  - 데이터 삭제
    - V remove(Object key) : key에 해당하는 key와 value를 삭제한다.
    - boolean remove(Object key, Object value) : key와 value 둘 다 일치하는 값을 삭제한다.

  - 데이터 수정
    - V replace(K key, V value) : key와 일치하는 value를 수정한다.
    - boolean replace(K key, V oldValue, V newValue) : key와 oldValue가 일치하는 값을 newValue로 수정한다.

  - 데이터 검색
    - V get(Object key) : key에 해당하는 value를 검색한다.
    - V getOrDefault(Object key, V defaultValue) : key에 해당하는 value가 있으면 값을 가져오고 없으면 defaultValue를 가져온다.
    - Set<Map.Entry<K, V>> entrySet() : 모든 key, value 데이터를 가져온다.
    - Set<K> keySet() : 모든 key 데이터를 가져온다.
    - Collection<V> values() : 모든 value 데이터를 가져온다.

  - 데이터 확인
    - boolean containsKey(Object key) : key가 포함되어 있는지 확인한다.
    - boolean containsValue(Object value) : value가 포함되어 있는지 확인한다.
    - boolean isEmpty() : 비어있는지 확인한다.
    - int size() : 길이를 확인한다.

### 참고 사이트
- https://kadosholy.tistory.com/120
