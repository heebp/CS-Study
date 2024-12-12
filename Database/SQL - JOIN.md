# SQL - JOIN

## JOIN의 개념

JOIN(조인)은 2개의 테이블에 대해 연관된 튜플들을 결합하여, 하나의 새로운 릴레이션을 반환

- JOIN은 크게 **INNER JOIN**과 **OUTER JOIN**으로 구분

- JOIN은 일반적으로 FROM절에 기술하지만, 릴레이션이 사용되는 어느 곳에서나 사용 가능

![JOIN](/Database/images/join.png)

    📌 JOIN을 사용하는 이유?

    관계형 데이터베이스는 중복 데이터를 방지하기 위해 여러 테이블로 나누어 데이터를 관리함

    그러므로 하나의 데이터를 찾기 위해 여러 테이블을 참조해야하는 경우가 필요하므로 정규화를 위한 JOIN을 사용

<br>

## 1. INNER JOIN

INNER JOIN은 키 값이 있는 테이블의 컬럼 값을 비교 후 조건에 맞는 값을 가져오는 것 (교집합)

![Inner Join](/Database/images/innerjoin.png)

```sql
-- 암시적 조인 표현
SELECT 조회할 컬럼
FROM 테이블1, 테이블2
WHERE 조건문

-- 명시적 조인 표현
SELECT 조회할 컬럼
FROM 테이블1 INNER JOIN 테이블2
ON 테이블1.컬럼 = 테이블2.컬럼
WHERE 조건문
```

![Inner Join Example](/Database/images/innerjoinexample.png)

```sql
SELECT SALES.*, COUNTRIES.Country
FROM SALES
JOIN COUNTRIES
ON SALES.CountryID = COUNTRIES.ID
```

### 1-1. EQUI JOIN(동등조인)

WHERE 절에 기술되는 JOIN 조건을 검사해서 양쪽 테이블에 같은 조건의 값이 존재할 경우 해당 데이터를 가져오는 조인 방법

```sql
SELECT *
FROM EMPLOYEE, DEPARTMENT
WHERE EMPLOYEE.DepartmentID = DepartmentID;

SELECT *
FROM EMPLOYEE JOIN DEPARTMENT
ON EMPLOYEE.DepartmentID = DEPARTMENT.DepartmentID;
```

### 1-2. CROSS JOIN(교차조인)

Cartesian Product(카디션 곱)이라고도 하며, 조인되는 두 테이블에서 곱집합을 반환

> 예를 들어, M열을 가진 테이블과 N열을 가진 테이블이 CROSS JOIN 될 경우 M\*N 개의 열을 생성하는 것으로, **테이블의 각 값을 연결하여 테이블 행의 수를 모두 곱한 값만큼 생성**

![Cross Join](/Database/images/crossjoin.png)

```sql
SELECT *
FROM EMPLOYEE CROSS JOIN DEPARTMENT
```

<br>

## 2. OUTER JOIN

OUTER JOIN은 조인하는 여러 테이블에서 한 쪽에는 데이터가 있고 한 쪽에는 데이터가 없는 경우, 데이터가 있는 쪽 테이블의 내용을 전부 출력하는 방법

> 대표적으로 LEFT OUTER JOIN, RIGHT OUTER JOIN, FULL OUTER JOIN이 있다

### 2-1. LEFT OUTER JOIN

LEFT OUTER JOIN은 조인문의 **왼쪽**에 있는 테이블의 결과를 가져온 후 **오른쪽** 테이블의 데이터를 매칭하고, 매칭되는 데이터가 없는 경우 NULL을 표시

![Left Outer Join](/Database/images/leftouterjoin.png)

```sql
-- LEFT OUTER JOIN
SELECT 조회할 컬럼
FROM 기준 테이블1 LEFT OUTER JOIN 테이블2
ON 조건문
WHERE 추가 조건문

-- 사용 예시
SELECT *
FROM EMPLOYEE E LEFT OUTER JOIN DEPARTMENT D
ON E.DepartmentID = D.DepartmentID
```

### 2-2. RIGHT OUTER JOIN

RIGHT OUTER JOIN은 조인문의 **오른쪽**에 있는 테이블의 모든 결과를 가져온 후 **왼쪽**의 테이블의 데이터를 매칭하고, 매칭되는 데이터가 없는 경우 NULL을 표시

![Right Outer Join](/Database/images/rightouterjoin.png)

```sql
-- RIGHT OUTER JOIN
SELECT 조회할 컬럼
FROM 테이블1 RIGHT OUTER JOIN 기준 테이블2
ON 조건문
WHERE 추가 조건문

-- 사용 예시
SELECT *
FROM EMPLOYEE E RIGHT OUTER JOIN DEPARTMENT D
ON E.DepartmentID = D.DepartmentID
```

### 2-3. FULL OUTER JOIN

FULL OUTER JOIN은 LEFT OUTER JOIN과 RIGHT OUTER JOIN을 합친 것으로, 양쪽 모두 조건이 일치하지 않는 것들까지 모두 결합하여 출력

![Full Outer Join](/Database/images/fullouterjoin.png)

```sql
-- FULL OUTER JOIN
SELECT 조회할 컬럼
FROM 테이블1 FULL OUTER JOIN 테이블2
ON 조건문
WHERE 추가 조건문

-- 사용 예시
SELECT *
FROM EMPLOYEE E FULL OUTER JOIN DEPARTMENT D
ON E.DepartmentID = D.DepartmentID
```

    📌 MySQL에서의 예외 사용 방법

    MySQL에서는 FULL OUTER JOIN을 지원하지 않으므로 LEFT OUTER JOIN 결과와 RIGHT OUTER JOIN 결과를 UNION 하여 사용

<br>

## 3. SELF JOIN

SELF JOIN은 같은 테이블에서 2개의 속성을 연결하여 EQUI JOIN을 하는 방법 (즉, 자기자신을 조인을 시키는 것)

```sql
SELECT A.컬럼, A.컬럼, B.컬럼 AS 별칭
FROM 테이블1 AS A JOIN 테이블2 AS B
ON A.컬럼 = B.컬럼
```

### SELF JOIN을 사용하는 이유

https://kimsyoung.tistory.com/entry/SELF-JOIN-%E4%B8%8B-%EC%85%80%ED%94%84-%EC%A1%B0%EC%9D%B8%EC%9D%98-%EC%9A%A9%EB%A1%80

<br>

### 참고자료

https://datasagar.com/joins-in-sql/

https://jhkang-tech.tistory.com/55

https://boring9.tistory.com/51

https://devbattery.com/database/study-3/#db-join
