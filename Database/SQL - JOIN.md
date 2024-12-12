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

## INNER JOIN

INNER JOIN은 키 값이 있는 테이블의 컬럼 값을 비교 후 조건에 맞는 값을 가져오는 것 (교집합)

<p align="center">
  <img src="/Database/images/innerjoin.png" align="center">
</p>
![Inner Join](/Database/images/innerjoin.png)

### 참고자료

https://datasagar.com/joins-in-sql/
