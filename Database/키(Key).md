# 키(Key)

## 키(Key)의 개념 및 종류

키는 데이터베이스에서 조건에 만족하는 튜플을 찾거나 순서대로 정렬할 때 튜플들을 서로 구분할 수 있는 기준이 되는 속성(Attribute)을 의미

- **키의 종류**

1. **후보키**(Candidate Key) : **유일성**과 **최소성**을 만족하는 키

   - 유일성(Unique) : 하나의 키 값으로 튜플만을 유일하게 식별함

   - 최소성(Minimality) : 모든 레코드들을 유일하게 식별하는 데 꼭 필요한 속성으로만 구성

2. **기본키**(Primary Key) : 후보키 중에서 선택된 주키(Main Key). **NULL** 값이 들어갈 수 없으며, 기본키로 선택된 속성(Attribute)은 동일한 값이 들어갈 수 없음

3. **대체키**(Alternate Key) : 후보키가 둘 이상일 때 기본키를 제외한 나머지 후보키

4. **슈퍼키**(Super Key) : **유일성**을 만족하는 키. 릴레이션을 구성하는 모든 튜플에 대해 **유일성**은 만족시키지만, **최소성**은 만족시키지 못함

5. **외래키**(Foreign Key) : 다른 릴레이션의 기본키를 참조하는 속성 또는 속성들의 집합

6. **복합키**(Composite Key) : 2개 이상의 속성(Attribute)을 사용한 키

<br>

## 기본키(Primary Key)

테이블 내에서 특정 레코드(행)을 식별하기 위해 사용되며 중복되는 값을 가질 수 없음

- 기본키는 후보키의 성질을 포함

- NULL 값을 가질 수 없으며, 기본키로 설정된 속성에서는 NULL 값이 있어서는 안됨

<br>

## 후보키(Candidate Key)

테이블의 행을 유일하게 식별할 수 있는 속성 또는 속성의 집합

- 모든 릴레이션에는 반드시 하나 이상의 후보키가 존재

- 릴레이션에 있는 모든 튜플에 대해서 유일성과 최소성을 만족시켜야 함

<br>

## 대체키(Alternate Key)

후보키가 둘 이상일 때 기본키를 제외한 나머지 후보키의 집합

- 대체키의 경우 NULL 값을 가질 수 있음

![Alternate Key](/Database/images/alternatekey.png)

<br>

## 슈퍼키(Super Key)

행을 고유하게 식별할 수 있는 모든 필드의 조합

- 주키와 후보키, 대체키들의 조합이 슈퍼키가 됨

- 대체키가 포함되어 있으므로, NULL 값을 허용

![Super Key](/Database/images/superkey.png)

<br>

## 외래키(Foreign Key)

다른 릴레이션의 기본키를 참조하는 속성 또는 속성들의 집합

- 두 개 이상의 테이블을 연결하는데 사용

- 외래키로 지정되면 참조 릴레이션의 기본키에 없는 값은 입력 불가

![Foreign Key](/Database/images/foreignkey.png)

<br>

## 유니크키(Unique Key)

데이터베이스에서 테이블에 존재하는 특정 컬럼(열)의 값이 모두 다르도록 제약을 걸어둔 것

- 테이블 내에는 여러 개의 유니크키가 존재 가능

- 유니크 키는 빈값 또는 NULL 값을 허용하는 데이터베이스도 있으나, NOT NULL 제약 조건을 걸어두는 것을 권장

![Unique Key](/Database/images/uniquekey.png)

<br>

### 참고자료

https://www.geeksforgeeks.org/types-of-keys-in-relational-model-candidate-super-primary-alternate-and-foreign/

https://adjh54.tistory.com/245

https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%ED%82%A4KEY-%EC%A2%85%EB%A5%98-%F0%9F%95%B5%EF%B8%8F-%EC%A0%95%EB%A6%AC
