# JPA(Java Persistence API): ORM 기술의 표준 인터페이스

## JPA란?
- **ORM(Object-Relational Mapping)**: 객체와 관계형 데이터베이스를 매핑해 주는 기술.
- **JPA**: ORM 기술을 **표준화**한 인터페이스로, 자바 애플리케이션에서 관계형 데이터베이스를 사용할 때 이를 객체와 매핑하여 사용.
- JPA는 인터페이스이므로 구현체가 필요하며, 대표적인 구현체로는 **Hibernate**, **EclipseLink**, **DataNucleus** 등이 있다.  
  - 이 중, **Hibernate**가 가장 널리 사용됨.

---

## JPA 사용의 필요성
### 객체 지향 프로그래밍과 관계형 데이터베이스의 문제
- 객체 지향 프로그래밍은 **다형성**, **상속** 등의 특성을 활용해 유연하고 확장 가능한 코드를 작성.
- 하지만 객체의 관계를 관계형 데이터베이스 테이블에 저장하고 관리하는 것은 복잡하고 어렵다.

### JPA의 역할
- **객체와 데이터베이스 테이블 간의 매핑 설정**만으로 데이터베이스 작업을 처리.
- 개발자는 **객체 지향적인 코드 작성**에 집중 가능.
- 데이터베이스와의 **패러다임 불일치 문제**를 해결.

---

## JPA 사용 예시

### JPA 없이 데이터베이스 작업
```java
public class UserDao {
    private DataSource dataSource;

    public UserDao(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public void addUser(User user) throws SQLException {
        String sql = "INSERT INTO users (username, email) VALUES (?, ?)";
        try (Connection conn = dataSource.getConnection();
             PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, user.getUsername());
            pstmt.setString(2, user.getEmail());
            pstmt.executeUpdate();
        }
    }

    public User getUser(String username) throws SQLException {
        String sql = "SELECT * FROM users WHERE username = ?";
        try (Connection conn = dataSource.getConnection();
             PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, username);
            ResultSet rs = pstmt.executeQuery();
            if (rs.next()) {
                return new User(rs.getString("username"), rs.getString("email"));
            }
            return null;
        }
    }
}
```
위의 코드에서 볼 수 있듯이, 각각의 데이터베이스 연산을 위해 SQL 쿼리를 직접 작성해야 한다. 이는 시간이 많이 걸릴 뿐 아니라 오류가 발생하기도 쉽다.
이때 JPA를 사용해 엔티티 클래스를 정의하면(위에서 서술한 대로 JPA를 사용했다는 것은 구현했다는 뜻이니, 정확히 말하면 Hibernate를 사용한 것이다.)

---

### JPA를 사용한 데이터베이스 작업

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "username")
    private String username;

    @Column(name = "email")
    private String email;

    // Constructor, Getter, Setter 생략
}

public class UserDao {
    @PersistenceContext
    private EntityManager em;

    public void addUser(User user) {
        em.persist(user);
    }

    public User getUser(String username) {
        return em.createQuery("SELECT u FROM User u WHERE u.username = :username", User.class)
                .setParameter("username", username)
                .getSingleResult();
    }
}
```

SQL 쿼리를 직접 작성하는 대신 객체와 데이터베이스 테이블 간의 매핑을 정의하고, 간단한 메서드 호출로 데이터베이스 작업을 수행할 수 있다.
이는 편리할 뿐만 아니라, 코드의 가독성과 유지보수성도 높일 수 있다.

## JPA의 장점

### 1. **객체와 데이터베이스 간 패러다임 불일치 해결**
- 테이블 간의 매핑을 설정하는 것만으로 JPA가 객체의 상태를 데이터베이스에 맞춰 저장하고 관리.
- 개발자는 **객체 중심 코드 작성**에 집중할 수 있음.

---

### 2. **생산성 향상**
- CRUD 작업을 위한 기본 SQL 쿼리를 **자동 생성**.
- 복잡한 SQL 쿼리를 직접 작성할 필요 없음.
- 간단한 메서드 호출로 데이터베이스 작업 수행 가능.
- 👉 **개발 시간 단축, 생산성 향상**

---

### 3. **유지보수성 향상**
- SQL 쿼리가 여러 곳에 흩어져 있다면, 데이터베이스 구조 변경 시 모든 관련 SQL 쿼리를 수정해야 하는 부담이 큼.
- JPA는 데이터베이스 구조가 변경되어도 대부분의 경우 이를 자동으로 처리.
- 객체 중심 코드로 가독성과 이해도가 높음.
- 👉 **유지보수성 향상**

---

### 4. **데이터베이스 독립성**
- 기존 SQL 쿼리는 특정 데이터베이스에 종속적일 수 있음.
- JPA는 데이터베이스에 **독립적**이므로, 데이터베이스 변경 시 코드 수정이 최소화됨.

---

### 5. **성능 최적화 기능**
- JPA는 다양한 성능 최적화 기능을 제공하여 불필요한 데이터베이스 접근을 줄이고 애플리케이션 성능을 향상.

#### **지연 로딩(Lazy Loading)**
- 필요할 때만 관련 엔티티를 로딩.
- 예: 사용자 정보 조회 시, 사용자의 모든 게시물을 즉시 로딩하지 않고, 게시물 정보가 필요할 때 데이터베이스에서 조회.
- 👉 **초기 로딩 시간 단축, 리소스 사용 최적화**

#### **1차 캐시(First-Level Cache)**
- 영속성 컨텍스트 내에 1차 캐시 유지.
- 같은 트랜잭션에서 동일한 엔티티를 반복 조회할 때 데이터베이스에 여러 번 접근하지 않음.
- 👉 **데이터베이스 부하 감소, 성능 향상**

#### **쓰기 지연(Write Behind)**
- 트랜잭션 커밋 전까지 변경된 엔티티를 메모리에 보관.
- 커밋 시점에 한 번에 변경 사항을 데이터베이스에 반영.
- 👉 **데이터베이스 연결 및 데이터 전송 최소화**

#### **배치 처리(Batch Processing)**
- 여러 삽입, 업데이트, 삭제 쿼리를 하나의 배치 작업으로 묶어 처리.
- 👉 **네트워크 비용 감소, 데이터베이스 처리 성능 향상**

#### **쿼리 캐시(Query Cache)**
- 자주 사용되는 쿼리의 결과를 캐시에 저장.
- 동일한 쿼리 요청이 있을 경우, 캐시에서 결과를 바로 가져옴(데이터베이스 접근 필요 없음).
- 👉 **쿼리 처리 속도 향상**

## JPA의 동작 원리

1. **애플리케이션과 JDBC 사이에서 동작**
   - JPA는 **JDBC API**를 사용하여 데이터베이스와 통신.

2. **Entity 객체 분석 및 SQL 생성**
   - 애플리케이션에서 `persist()` 메서드를 호출하면, JPA는 다음 단계를 수행:
     - **Entity 객체 분석**: 객체의 상태와 매핑 정보를 분석.
     - **SQL 생성**: 적절한 SQL 쿼리를 생성.

3. **SQL 실행**
   - 생성된 SQL은 **JDBC API**를 통해 데이터베이스로 전송 및 실행.

4. **객체-관계 매핑**
   - 데이터베이스에서 실행된 결과를 다시 객체로 매핑하여 애플리케이션에서 사용할 수 있도록 관리.

# 하이버네이트(Hibernate)

**하이버네이트(Hibernate)**는 Java 언어를 위한 ORM 프레임워크로, **JPA 인터페이스를 구현한 구현체**입니다.

- 내부적으로 **JDBC API**를 사용하며, JPA의 특징과 장점을 그대로 가지고 있습니다.
- 이를 통해 개발자가 JPA를 더욱 편리하게 사용할 수 있도록 지원합니다.

---

## Hibernate 주요 특징

하이버네이트는 JPA의 구현체로, JPA의 특징을 모두 가지며 추가적인 기능도 제공합니다. 주요 특징은 다음과 같습니다:

1. **ORM 기능**
   - 객체와 관계형 데이터베이스 간의 매핑을 지원.

2. **JDBC 추상화**
   - JDBC API를 추상화하여 사용자의 작업을 간소화.

3. **다양한 쿼리 언어 지원**
   - **JPQL(Java Persistence Query Language)**, 네이티브 SQL, **Querydsl** 등 다양한 쿼리 언어 지원.

4. **캐싱 기능**
   - **1차 캐시** 및 **2차 캐시**를 통해 데이터베이스 접근을 최적화.

5. **데이터베이스 독립성**
   - 데이터베이스에 종속되지 않으며, 여러 데이터베이스에서 동일한 코드 사용 가능.

---

## Hibernate 사용 방법

하이버네이트를 사용하는 기본적인 방법을 알아보겠습니다.

### 1. **엔티티 클래스 정의**
- JPA 어노테이션을 사용해 데이터베이스 테이블과 매핑되는 **엔티티 클래스**를 정의합니다.
- 주요 어노테이션:
  - `@Entity`: 엔티티 클래스를 선언.
  - `@Table`: 데이터베이스 테이블과 매핑.
  - `@Id`: 기본 키를 설정.
  - `@Column`: 필드와 테이블의 컬럼을 매핑.

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "name")
    private String name;
    
    @Column(name = "email")
    private String email;
    
    // Getter, Setter, Constructor 생략
}
```

### 2. **EntityManager 생성**
- **EntityManagerFactory**를 통해 **EntityManager**를 생성합니다.
- **EntityManager**는 데이터베이스 연결과 트랜잭션 관리를 수행합니다.

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("myPersistenceUnit");
EntityManager em = emf.createEntityManager();
```

### 3. **CRUD 작업 수행**

`EntityManager`를 사용하여 엔티티에 대한 CRUD 작업을 수행할 수 있습니다.

아래는 위에서 정의한 `user` 테이블에 대한 CRUD 작업 구현 예시입니다:

```java
// Create
em.getTransaction().begin();
User user = new User("Chaeyami", "chaeyami@example.com");
em.persist(user);
em.getTransaction().commit();
 
// Read
User foundUser = em.find(User.class, user.getId());
 
// Update
em.getTransaction().begin();
foundUser.setEmail("chaeyami02@example.com");
em.merge(foundUser);
em.getTransaction().commit();
 
// Delete
em.getTransaction().begin();
em.remove(foundUser);
em.getTransaction().commit();
```

### 4. **쿼리 작성**

Hibernate는 **JPQL**, **네이티브 SQL**, **Querydsl** 등을 지원하여 데이터베이스 작업을 유연하게 처리할 수 있습니다.

아래는 `user` 테이블에서 모든 컬럼을 가져오는 **JPQL**과 **네이티브 SQL** 예시입니다:

```java
// JPQL
List<User> users = em.createQuery("SELECT u FROM User u", User.class).getResultList();
 
// Native SQL
List<User> users = em.createNativeQuery("SELECT * FROM users", User.class).getResultList();
```
