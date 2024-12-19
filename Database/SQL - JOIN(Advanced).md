# SQL - JOIN(Advanced)

## JOIN 요약 (종류 및 방식)

JOIN(조인)은 2개의 테이블에 대해 연관된 튜플들을 결합하여, 하나의 새로운 릴레이션을 반환

    📌 JOIN을 사용하는 이유?

    관계형 데이터베이스는 중복 데이터를 방지하기 위해 여러 테이블로 나누어 데이터를 관리함

    그러므로 하나의 데이터를 찾기 위해 여러 테이블을 참조해야하는 경우가 필요하므로 정규화를 위한 JOIN을 사용

### JOIN의 종류

**INNER JOIN** (EQUI JOIN, CROSS JOIN)
<br>
**OUTER JOIN** (LEFT OUTER JOIN, RIGHT OUTER JOIN, FULL OUTER JOIN)
<br>
**SELF JOIN**

### JOIN의 방식

    💡 JOIN은 상당한 시간이 걸릴 수 있기에 내부적으로 다양한 구현 방식을 사용하는데, 어떤 방식들이 있나요?

일반적으로 옵티마이저가 계획을 세워 동작을 수행하지만, 항상 최적화된 계획만을 세우는 것이 아님
<br>
이러한 경우에 수동적으로 JOIN 계획을 세울 필요가 있음

<br>

## 1. Nested Loop Join (NL Join, 중첩 루프 조인)

가장 기본적인 JOIN 알고리즘으로, 두 테이블을 중첩 반복문처럼 순차적으로 비교하는 방식

- 조인하려는 두 개의 테이블 중 더 작은 테이블을 선행 테이블(Outer Table)이라 하고, 더 큰 테이블을 후행 테이블(Inner Table)이라고 함

- 선행 테이블(Outer Table)이 작고, 후행 테이블(Inner Table)이 커야 효과적

![Nested Loop Join](/Database/images/nested_loop_join.png)

1. 선행 테이블에서 조건을 만족하는 첫 번째 행을 찾음

2. 선행 테이블의 조인 키를 가지고 후행 테이블에 있는 조인 키가 존재하는지 찾음

3. 후행 테이블의 인덱스에 선행 테이블의 조인 키가 존재하는지 확인

4. 인덱스에서 추출한 레코드 식별자를 이용하여 후행 테이블을 액세스

   > 후행 테이블의 조건까지 만족하면 해당 행을 추출버퍼에 삽입

5. 앞의 작업을 반복 수행

### 1-1. **Nested Loop Join 코드 예시**

```sql
-- Oracle 코드
SELECT /*+ USE_NL (B) */ A.*, B.*
FROM ITEM A, UITEM B
WHERE A.ITEM_ID = B.ITEM_ID -- 1
AND A.ITEM_TYPE_CD = '100100' -- 2
AND A.SALE_YN = 'Y' -- 3
AND B.SALE_YN = 'Y' -- 4

ITEM_X01 -> ITEM_TYPE_CD
UITEM_PK -> ITEM_ID + UITEM_ID

-- MySQL 코드
SELECT A.*, B.*
FROM ITEM A
STRAIGHT_JOIN UITEM B  -- Nested Loop Join 유도
ON A.ITEM_ID = B.ITEM_ID  -- 1
WHERE A.ITEM_TYPE_CD = '100100' -- 2
  AND A.SALE_YN = 'Y' -- 3
  AND B.SALE_YN = 'Y' -- 4
```

![Nested Loop Join Example](/Database/images/nested_loop_join_example.png)

### 1-2. **Nested Loop Join의 특징**

- 랜덤 액세스 위주의 조인 방식으로, 인덱스 구성이 아무리 완벽하더라도 대량의 데이터를 조인할 때 매우 비효율적

- 조인을 한 레코드씩 순차적으로 진행

- 소량의 데이터를 주로 처리하거나 부분범위 처리가 가능한 온라인 트랜잭션 환경에 적합

  > 온라인 뱅킹, 쇼핑, 주문 입력 또는 텍스트 메세지 전송 등 동시에 발생하는 다수의 트랜잭션을 실행하는 데이터 처리 유형

<br>

## 2. Sort Merge Join (병합 조인)

두 테이블을 JOIN 키를 기준으로 오름차순 정렬한 후 병합하는 방식

- 조인 키에 인덱스가 없는 경우에도 사용 가능

- 두 테이블의 크기가 비슷한 경우 유리하고, 크기기 차이가 큰 경우 불리하고 비효율적임

- 조인 조건으로 >, <, <=, >=와 같은 범위 비교 연산자가 많이 사용된 경우 사용

![Sort Merge Join](/Database/images/sort_merge_join.png)

1. 선행 테이블에서 주어진 조건을 만족하는 행을 찾음

2. 선행 테이블의 조인 키를 기준으로 정렬 작업을 수행

   > 위 작업을 선행 테이블의 조건을 만족하는 모든 행에 대해 반복 수행

3. 후행 테이블에서 주어진 조건을 만족하는 행을 찾음

4. 후행 테이블의 조인 키를 기준으로 정렬 작업을 수행

   > 위 작업을 후행 테이블의 조건을 만족하는 모든 행에 대해 반복 수행

5. 정렬된 결과를 이용하여 조인을 수행하며 조인에 성공하면 추출버퍼에 삽입

### 2-1. **Sort Merge Join 코드 예시**

```sql
-- Oracle 코드
SELECT /*+ ORDERED USE_MERGE(B) */ A.*, B.*
FROM ITEM A, UITEM B
WHERE A.ITEM_ID = B.ITEM_ID -- 1
AND A.ITEM_TYPE_CD = '100101' -- 2
AND A.SALE_YN = 'Y' -- 3
AND B.SALE_YN = 'Y' -- 4

ITEM_X01 -> ITEM_TYPE_CD UITEM -> 없음

-- MySQL 코드
SELECT /*+ MERGE JOIN */ A.*, B.*
FROM (
        SELECT *
        FROM ITEM
        WHERE ITEM_TYPE_CD = '100101' -- 2
          AND SALE_YN = 'Y' -- 3
        ORDER BY ITEM_ID -- 병합 조인을 위해 정렬
    ) A
INNER JOIN (
        SELECT *
        FROM UITEM
        WHERE SALE_YN = 'Y' -- 4
        ORDER BY ITEM_ID -- 병합 조인을 위해 정렬
    ) B
ON A.ITEM_ID = B.ITEM_ID; -- 1
```

![Sort Merge Join Example](/Database/images/sort_merge_join_example.png)

### 2-2. **Sort Merge Join의 특징**

- 양 테이블을 동시에 읽고 양 테이블이 조인 준비가 되었을 때 조인 수행

- 정렬된 양쪽 결과를 스캔하는 방식이므로 인덱스 유무는 중요하지 않음

- 정렬된 테이블을 순차적으로 스캔하면서 매칭되는 레코드를 찾음

<br>

## 3. Hash Join (해시 조인)

두 테이블 중 하나를 해시 테이블로 선정하여, 테이블의 조인 키 값을 해시 알고리즘으로 비교하여 조인을 수행하는 방식

- **Sort Merge Join**은 정렬할 때 부하가 많이 발생하여, 이를 보완하기 위한 방법으로 정렬 대신 해시 값을 이용하는 조인

- 주로 많은 양의 데이터를 조인해야 하는 경우에 사용

- 랜덤 액세스로 인해 조인 키에 적당한 인덱스가 없어 **Nested Loop Join**이 비효율적일 때 사용

- 비용 기반 옵티마이저를 사용할 때만 사용될 수 있는 조인 방식으로, '=' 비교를 통한 조인에서만 사용 가능

![Hash Join](/Database/images/hash_join.png)

1. 선행 테이블에서 주어진 조건을 만족하는 행을 찾음

2. 선행 테이블의 조인 키를 기준으로 해시 함수를 적용하여 해시 테이블을 생성

   > 위 작업을 선행 테이블의 조건을 만족하는 모든 행에 대해 반복 수행

3. 후행 테이블에 주어진 조건을 만족하는 행을 찾음

4. 후행 테이블의 조인 키를 기준으로 해시 함수를 적용하여 해당 버킷을 찾음

5. 조인에 성공하면 추출버퍼에 삽입

   > 위 작업을 후행 테이블의 조건을 만족하는 모든 행에 대해 반복 수행

### 3-1. Hash Join의 종류

1. **In-Memory Hash Join** (인 메모리 해시 조인) : 해시 테이블을 메모리에 유지할 수 있는 경우의 조인 방식

2. **Grace Hash Join** (유예 해시 조인) : 해시 테이블을 저장할 메모리가 부족해서 디스크가 필요한 경우의 조인 방식

3. **Recursive Hash Join / Nested Loop Hash Join** (재귀 해시 조인) : 더 작은 파티션을 메모리에 올리는 과정에서 또 메모리를 초과하는 경우 사용

4. **Hybrid Hash Join** : In-Memory Hash Join과 Grace Hash Join 요소가 결합되어 조인되는 방식

<br>

## JOIN 방식 비교 정리

| JOIN 방식        | 장점                           | 단점                     | 적합한 상황              |
| ---------------- | ------------------------------ | ------------------------ | ------------------------ |
| Nested Loop Join | 간단한 알고리즘, 작은 데이터셋 | 큰 데이터셋에서 비효율적 | OLTP 환경                |
| Sort Merge Join  | 정렬 없이 처리 가능            | 정렬 비용 발생           | 정렬된 데이터, 범위 조건 |
| Hash Join        | 대용량 데이터셋 효율적 처리    | 메모리 제한시 성능 저하  | '=' 조건, OLAP 환경      |

<br>

### 참고 자료

https://land-turtler.tistory.com/121

https://devbattery.com/java/study-12/

https://ryean.tistory.com/73

https://cafe.naver.com/sqlpd

https://velog.io/@invidam/Join-%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9D

https://beom92.tistory.com/9
