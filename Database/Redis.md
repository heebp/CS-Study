# Redis

- Remote Dictionary Server
- **키-값** 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 **비관계형** 데이터베이스 관리 시스템
- **데이터 속도 처리가 엄청 빠른 NoSQL 데이터베이스**

<br>

## Redis 장점

- 인메모리(in-memory)에 모든 데이터를 저장 ➡ **데이터 처리 성능 굉장히 빠름**

> MySQL 같은 RDBMS의 데이터베이스는 대부분 **디스크(Disk)**에 저장 <br>
> 하지만 Redis는 **메모리(RAM)**에 저장
> ➔ 데이터 처리 속도 : 메모리(RAM) > 디스크(Disk)

### 🤔기존 DB가 있는데 Redis를 사용하는 이유?

- DB는 서버에 문제가 발생하여 다운되더라도 데이터 손실이 없지만 매번 디스크에 접근해야하지 때문에 사용자가 많아질 수록 부하가 많아져서 느려질 수 있음.
- 따라서 캐시 서버를 도입해야함 = **Redis**

<br>

## Redis 특징

- Key, Value 구조
- 빠른 처리 속도
- Data Type(Collection)을 지원
  ![](https://velog.velcdn.com/images/wnguswn7/post/dbd60200-7704-4543-a2b3-d2e171755ed5/image.PNG)
  > DB에 저장하고 저장된 데이터를 정렬해서 다시 읽어오려면 Redis에서 제공하는 Sorted-Set 자료구조를 사용
- 인메모리 데이터 저장소가 가지는 휘발성의 특성으로 데이터가 유실될 경우를 방지하여 백업 기능을 제공
  - AOF (Append On File) : Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록
  - RDB (Snapsghotting) : 순간적으로 메모리에 있는 내용 전체를 디스크에 담아 영구 저장
- 자동 파티셔닝 제공
  - 다수의 Redis가 존재할 때 데이터를 여러 곳으로 분산시키는 기술
  - 각 Redis 인스턴스는 전체 키 중 자신에게 할당된 일부 파티션의 키들만 관리
  - Redis Sentinel 및 Redis Cluster 사용
  - Master와 Slaves로 구성하여 여러 대의 복제본을 만들 수 있고 여러 대의 서버로 읽기 확장 가능
- 다양한 프로그래밍 언어 지원
- 싱글 스레드
  - 한번에 하나의 명령만 수행 가능하므로 Race Condition 거의 발생 X
  - Race Condition : 공유 자원에 대해 여러 프로세스가 동시에 접근을 시도할 때 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태

<br>

## Redis 사용 시 주의할 점

- 시간 복잡도

  - 싱글 스레드 사용으로 한번에 하나의 명령만 수행이 가능하므로 처리 시간이 긴 요청의 경우 장애 발생

- 메모리 단편화
  - 크고 작은 데이터를 할당하고 해제하는 과정에서 메모리 파편화가 발생하여 응답 속도가 느려짐
    ![](https://velog.velcdn.com/images/wnguswn7/post/28d97f91-94a8-45d2-8c73-23c36afdde4e/image.png)
    - RAM에서 메모리를 할당받고 해제하는 과정에서 부분적으로 빈 공간이 생겨, 새로운 메모리 할당 시 사용 가능한 메모리가 충분히 존재하지만 메모리의 크기만큼의 부분이 없어 마지막 부분에 할당되어 메모리 낭비가 심하게 됨 <br>
      ➜ 이 현상이 계속되면 실제 physical 메모리가 커져 프로세스가 죽는 현상이 발생 할 수도 있으므로, redis를 사용 시에 메모리를 적당히 여유있게 사용하는 것이 좋음
- 주기적인 모니터링
  - 메모리 사용량이 너무 많으면 Redis 서버의 성능 저하나 장애로 이어질 수 있음

<br>

## Redis 주요 사용 사례

- **캐싱 (Cashing)**
- 세션 관리 (Session Management)
- 실시간 분석 및 통계 (Real-time Analystics)
- 메세지 큐 (Message Queue)
- 지리공간 인덱싱 (Geospatial Indexing)
- 속도 제한 (Rate Limiting)
- 실시간 채팅 및 메시징 (Real-time Chat And Messaging)

> **대용량 트래픽 처리하기 위해 필수적으로 사용되는 기능이 Redis의 캐싱(Caching)**

<br>

### 캐싱

> - 캐시(Cache) : 원본 저장소보다 빠르게 가져올 수 있는 임시 데이터 저장소
> - 캐싱(Caching) : 캐시에 접근해서 데이터를 빠르게 가져오는 방식

#### 1. Cache Aside (=Look Aside, Lazy Loading) 전략 (조회)

- 캐시O : Cache Hit
- 캐시X : Cache Miss
  ![](/Database/images/redis.jpg)
  ![](/Database/images/redis2.jpg)

#### 2. Write Around 전략 (저장, 수정, 삭제)

![](/Database/images/redis3.jpg)

<br>

- 같이 썼을 때 한계점

  - 캐시된 데이터와 DB 데이터가 일치하지 않을 수 없음 (데이터 일관성 보장X) <br>
    ➔ 데이터를 수정할 때마다 동시에 업데이트 시켜 캐시와 DB 데이터 일치시킴 (BUT 성능 느려짐)

    - 조회 성능 개선 목적으로 레디스를 쓰는 경우는 데이터 일관성을 포기하고 성능 향상을 택함
    - 캐시를 적용 시키기 적절한 데이터 : 자주 조회되는 데이터, 잘 변하지 않는 데이터, 실시간으로 정확하지 않아도 되는 데이터
    - 하지만 장기간 데이터 불일치는 문제가 됨 → 적절한 주기로 동기화 시켜야 함 <br>
      ⇨ **TTL기능** **(만료 시간 설정)** 사용

  - 캐시에 저장할 수 있는 공간이 비교적 작음
    ⇨ **TTL기능** **(만료 시간 설정)** 사용
