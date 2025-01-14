Eager, Lazy Loading
=============

## 1. Eager Loading
> Eager Loading이란?
  - 즉시 로딩은 JPA에서 연관된 모든 테이블을 조인을 통해 조회하는 것을 의미한다.

> Eager Loading 사용 방법
  - 엔티티에 FetchType을 Eager로 설정할 수 있으며 디폴트가 Eager이다.
  ![eager 이미지](/JPA/images/eager.png)

> Eager Loading 조회 결과

  ![eagerdb 이미지](/JPA/images/eagerdb.png)
  - 현재 File 엔티티는 Member 엔티티와 일대일 관계이다.
  - Eager Loading으로 설정하면 File 엔티티만 조회해도 관련된 Member 엔티티도 조인되어 같이 조회된다.

## 2. Lazy Loading
> Lazy Loading이란?
  - 지연 로딩은 JPA에서 연관된 모든 테이블을 실제 사용할 때 조회하는 것을 의미한다.

> Lazy Loading 사용 방법
  - 엔티티에 FetchType을 Lazy로 설정한다.
  ![lazy 이미지](/JPA/images/lazy.png)

> Lazy Loading 조회 결과

  ![lazydb 이미지](/JPA/images/lazydb.png)
  - Lazy Loading으로 설정하면 File 엔티티만 조회된다.

> Eager, Lazy 장단점
  - Eager Loading은 연관된 엔티티를 모두 가져올 수 있지만 그만큼 성능에 영향을 미친다.
  - Eager Loading은 N + 1 문제를 발생시킬 위험이 있다.
  - N + 1을 간단히 설명하자면 하나의 테이블을 모두 조회할 때 각 로우당 연관되어 있는 모든 테이블을 가져오기 때문에 발생하는 문제이다.
  - Lazy Loading은 불필요한 엔티티를 조회하는 경우가 없어 로딩 시간을 감소시킨다.

### 참고 사이트
- https://velog.io/@ssssujini99/SpringJPA-%EC%A6%89%EC%8B%9C-%EB%A1%9C%EB%94%A9Eager-Loading%EA%B3%BC-%EC%A7%80%EC%97%B0-%EB%A1%9C%EB%94%A9Lazy-Loading
- https://velog.io/@jinyoungchoi95/JPA-%EB%AA%A8%EB%93%A0-N1-%EB%B0%9C%EC%83%9D-%EC%BC%80%EC%9D%B4%EC%8A%A4%EA%B3%BC-%ED%95%B4%EA%B2%B0%EC%B1%85
