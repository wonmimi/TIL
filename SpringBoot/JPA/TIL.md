## JPA (Java Persistence API) 
- 객체지향 + ORM(Object Relational Mapping)
    - 객체지향 언어 (java, C++ .. )와 관계형 데이터베이스(Oracle, MySql ...)의 패러다임을 사이에서 일치시켜줌
    - 객체지향 프로그래밍을 하면서 JPA가 RDBMS에 맞게 SQL을 대신생성
      => sql에 종속되어 개발하지 않아도된다 (기존 sql Mapper 형태)
- 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해준다
- SQL과 데이터 중심의 설계 => 객체 중심의 설계 
- 인터페이스 제공. 구현체로 hibernate ... 여러개 있다.
- JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다 ( => 서비스 계층에 트랜잭션 추가 )
  - 스프링은 해당 클래스의 메서드를 실행할 때 트랜잭션을 시작하고, 메서드가 정상 종료되면 트랜잭션을커밋. 런타임 예외가 발생하면 롤백
- [`스프링 데이터 JPA`](./springDataJPA.md) 프레임워크를 더하면 , 리포지토리에 구현 클래스 없이 인터페이스 만으로 개발 하여 반복코드를 줄일 수 있다. (jpa 선행 필수)
- JPA (인터페이스) <- Hibernate (구현체) <- Spring Data JPA (구현체 추상화)
  - 구현체, 저장소(DB) 교체가 용이해서 Spring Data JPA 사용
  - Spring Data의 하위 프로젝트틀은 기본적인 CRUD 인터페이스가 동일 (save(), findAll() ... )
    => 하위 Ex) Spring Data JPA , Spring Data MongoDB , Spring Data Redis ... 
- 높은 트래픽 , 대용량 데이터 처리 서비스에 적격
-  Repository 는 엔티티 와 함께 위치해야한다 ( Mybatis에서 Dao(Data Access Object)와 동일)

> __관련 annotation__
- @Entity : 테이블과 링크될 클래스 , 클래스의 카멜케이스 이름을 언더스코어 네이밍 (camel_case) 으로 테이블 이름과 매칭. 
  - 엔티티 클래스는 Setter메소드를 만들지않는다
  - 기본적으로 생성자를 통해 값을 넣고, public 메소드를 호출하여 값 변경
- @Id : 해당 테이블의 PK필드
- @GeneratedValue(strategy = GenerationType.IDENTITY) : PK의 생성 규칙 
  - 2.0에서는 IDENTITY 추가해야 auto_increment
- @Column(length = 500, nullable = false)  : 테이블릐 컬럼. 선언하지 않아도 클래스의 필드는 모두 컬럼이 된다. 
  - 각 타입의 기본값 외에 추가 변경이 필요한 옵션이 있을경우 사용
  ```java
  @Getter
  @NoArgsConstructor //  기본생성자 자동추가
  @Entity 
  public class Posts {
      @Id 
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;

      @Column(length = 500, nullable = false)
      private String title;

      @Column(columnDefinition = "TEXT", nullable = false)
      private String content;

      private String author;

      @Builder
      public Posts(String title, String content, String author){
          this.title = title;
          this.content = content;
          this.author = author;
      }
  }
  ```
- @Query : springDataJPA 에서 제공하지 않는 메소드에 쿼리로 사용 
```java
   @Query("SELECT p FROM Posts p ORDER BY p.id DESC")
    List<Posts> findAllDesc();
```
- @Enumerated : 데이터베이스에 저장할 enum(열거형) 데이터 형태 지정
  - 기본적으로 int형 => DB에서 확인할떄 무슨코드인지 의미 알수 없다 
  - @Enumerated(EnumTypes.STRING)으로 문자열 선언


> 💻 셋팅 [정리](./setting.md) 

> __영속성 컨텍스트__

[`동일성`](https://velog.io/@neptunes032/JPA-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80) 보장 
