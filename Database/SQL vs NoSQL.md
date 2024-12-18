# SQL vs NoSQL

데이터베이스 관리 시스템(DBMS)에는 개발자와 기업들이 주로 사용하는 두 가지 유형이 있습니다: 
SQL(Structured Query Language) 데이터베이스와 NoSQL(Not Only SQL) 데이터베이스입니다. 
각 유형은 고유한 특성, 장점, 그리고 최적의 사용 사례를 가지고 있습니다. 

![image](https://github.com/user-attachments/assets/1777132b-56c1-4ca6-8f08-10ed73ba55aa)

---

## SQL
SQL, 즉 구조화 질의 언어(Structured Query Language)는 관계형 데이터베이스를 관리하고 조작하기 위해 특별히 설계된 표준 프로그래밍 언어입니다. 
이는 쿼리, 업데이트, 데이터 구조 관리 등 다양한 작업을 통해 관계형 데이터베이스 관리 시스템(RDBMS)에 저장된 데이터와 상호 작용하는 데 사용됩니다.

### SQL 데이터베이스의 특징
- 구조화된 데이터: 명확한 스키마를 가진 구조화된 데이터에 이상적입니다.
- ACID 준수: 원자성(Atomicity), 일관성(Consistency), 격리성(Isolation), 지속성(Durability)을 보장하여 신뢰할 수 있는 트랜잭션을 제공합니다.
- 스키마 기반: 데이터를 구성하기 위해 미리 정의된 스키마가 필요하며, 이는 엄격하지만 일관성 있는 구조를 만듭니다.
- SQL 언어: 데이터베이스 쿼리 및 유지 관리에 SQL을 사용합니다.
- 확장성: 일반적으로 수직적으로 확장됩니다.

### SQL 데이터베이스 카테고리
**1. 관계형 데이터베이스 관리 시스템(RDBMS)**
   
![image](https://github.com/user-attachments/assets/90e3a1f6-1557-45bc-b02d-76bc5bb8f0a9)

RDBMS는 데이터를 정의하고 조작하기 위해 구조화된 쿼리 언어(SQL)를 사용합니다. 
RDBMS는 데이터를 테이블에 저장하고 스키마를 사용하여 데이터 무결성과 테이블 간의 관계를 강제합니다.

예시: MySQL, PostgreSQL, Oracle Database, Microsoft SQL Server

사용 사례: 트랜잭션 애플리케이션, ERP 시스템, 고객 관계 관리(CRM), 재무 기록

**2. OLAP(Online Analytical Processing)**

![image](https://github.com/user-attachments/assets/36a67d27-c9f7-459b-9f5b-1aa356a50444)


OLAP 시스템은 복잡한 쿼리를 지원하도록 설계되었으며 데이터 분석과 비즈니스 인텔리전스에 사용됩니다. 
OLAP 시스템은 대량의 데이터를 처리할 수 있으며 분석 쿼리에 대해 빠른 응답 시간을 제공합니다.

예시: Microsoft SQL Server Analysis Services (SSAS), Oracle OLAP, SAP BW

사용 사례: 데이터 마이닝, 비즈니스 보고, 판매 및 마케팅 분석, 재무 예측

### SQL 데이터베이스를 사용해야 하는 경우
- 데이터가 구조화되어 있고 자주 변경되지 않습니다.
- 여러 행 트랜잭션과 복잡한 쿼리가 필요합니다.
- 데이터 무결성과 일관성이 중요합니다.
- 예측 가능하고 고정된 스키마가 있습니다.

---

## NoSQL
NoSQL은 "Not Only SQL"의 약자로, 전통적인 관계형 데이터베이스와 다른 광범위한 데이터베이스 관리 시스템을 의미합니다.
NoSQL 데이터베이스는 비구조화, 반구조화, 구조화된 데이터를 처리할 수 있도록 설계되었으며, 
SQL 데이터베이스보다 더 큰 유연성과 확장성을 제공합니다. 
이는 특히 대량의 데이터와 실시간 웹 애플리케이션을 처리하는 데 유용합니다.

### NoSQL 데이터베이스의 특징
- 유연한 스키마: 고정된 스키마 없이 비구조화, 반구조화 또는 구조화된 데이터를 처리할 수 있습니다.
- 최종 일관성: 즉각적인 일관성보다는 최종 일관성에 중점을 두어 높은 가용성을 제공합니다.
- 확장성: 수평적 확장성(부하를 분산하기 위해 더 많은 서버 추가)을 위해 설계되었습니다.
- 다양한 데이터 모델: 문서, 키-값, 칼럼 패밀리, 그래프 등 다양한 데이터 모델을 지원합니다.
- 성능: 특정 사용 사례에 최적화되어 대량의 데이터에 대해 더 빠른 읽기 및 쓰기 작업을 제공합니다.

### NoSQL 데이터베이스 카테고리
**1. 키-값 저장소(Key-Value Store)**
![image](https://github.com/user-attachments/assets/6ccff0ce-597f-4c9f-a26f-0ff41034832c)

키-값 저장소는 간단한 키-값 방식을 사용하여 데이터를 저장하는 NoSQL 데이터베이스 유형입니다.
각 데이터 항목은 키와 관련 값으로 저장되며, 이는 사전이나 해시 테이블과 유사합니다.
키-값 저장소는 단순성과 키를 알고 있을 때 빠른 값 검색을 위해 설계되었습니다.

예시: Redis, DynamoDB, Riak

사용 사례: 캐싱, 세션 관리, 사용자 프로필, 구성 관리


**2. 문서 저장소(Document Store)**
![image](https://github.com/user-attachments/assets/5bbdcf42-b567-4209-9355-90fb2e5549fe)

문서 저장소는 문서 지향 정보를 저장, 검색 및 관리하도록 설계된 또 다른 유형의 NoSQL 데이터베이스입니다. 문서는 일반적으로 JSON, BSON 또는 XML과 같은 형식으로 저장되어 컬렉션 내에서 문서마다 다를 수 있는 유연한 스키마를 허용합니다.

예시: MongoDB, CouchDB, RavenDB

사용 사례: 콘텐츠 관리 시스템, 전자 상거래 애플리케이션, 실시간 분석

**3. 그래프 데이터베이스**
![image](https://github.com/user-attachments/assets/f068301b-d26e-4373-bdeb-ca84903876ac)

그래프 데이터베이스는 노드, 엣지, 속성을 사용하여 데이터를 표현하고 저장하는 의미론적 쿼리를 위해 그래프 구조를 사용합니다. 그래프 데이터베이스는 특히 엔티티 간의 관계를 탐색하는 데 적합합니다.

예시: Neo4j, ArangoDB, Amazon Neptune
사용 사례: 소셜 네트워크, 추천 엔진, 사기 감지, 네트워크 및 IT 운영

**4. 칼럼 저장소**
![image](https://github.com/user-attachments/assets/b1bd777f-b431-433c-bb3a-a6ef4a5a0625)
칼럼 저장소(또는 칼럼 패밀리 저장소)는 행이 아닌 열별로 데이터를 저장하는 NoSQL 데이터베이스 유형입니다. 이는 큰 데이터셋에 대한 집계와 요약이 일반적인 분석적 쿼리 워크로드에 특히 유리합니다.

예시: Apache Cassandra, HBase, Google Bigtable
사용 사례: 데이터 웨어하우징, 비즈니스 인텔리전스, 실시간 분석

## NoSQL 데이터베이스를 사용해야 하는 경우
- 대량의 비구조화 또는 반구조화된 데이터를 다루고 있습니다.
- 일관성보다는 확장성과 성능이 우선순위입니다.
- 애플리케이션이 변화하는 데이터 요구사항에 적응하기 위해 유연한 스키마가 필요합니다.
- 대규모 분산 데이터를 처리하고 있습니다.

---

## SQL과 NoSQL의 주요 차이점
1. 데이터 모델
SQL: 테이블, 행, 열이 있는 관계형
NoSQL: 다양한 데이터 모델(문서, 키-값, 칼럼 패밀리, 그래프)이 있는 비관계형
2. 스키마
SQL: 고정된 스키마; 미리 정의된 테이블과 열
NoSQL: 동적 스키마; 유연하고 적응 가능
3. 쿼리 언어
SQL: 데이터 쿼리에 SQL 사용
NoSQL: 데이터베이스 유형에 따라 다양함; 예를 들어 MongoDB는 JSON과 유사한 쿼리 사용
4. 확장성
SQL: 수직적 확장
NoSQL: 수평적 확장
5. 일관성
SQL: ACID 준수로 강력한 일관성 보장
NoSQL: 일반적으로 최종 일관성을 제공하며, 필요한 경우 더 강력한 일관성으로 조정 가능
6. 사용 사례
SQL: 여러 행 트랜잭션, 복잡한 쿼리, 일관성이 필요한 애플리케이션에 최적
NoSQL: 대규모 데이터 저장, 실시간 분석, 유연한 데이터 모델이 필요한 애플리케이션에 이상적

## 참고
https://maily.so/devpill/posts/wjzded2yo3p
https://www.mongodb.com/ko-kr/resources/basics/databases/nosql-explained/nosql-vs-sql
