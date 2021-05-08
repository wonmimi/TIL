### @ annotation 정리 
-  @Autowired : 생성자에 있으면 객체 생성 시점에 스프링이 연관된 객체를 스프링 컨테이너에서 해당 스프링빈을 찾아서 주입한다.
  객체 의존관계를 외부에서 넣어주는 것을 [DI (Dependency Injection)](./DI.md), 의존성 주입이라 한다.
    - 생성자가 1개만 있으면 @Autowired 는 생략할 수 있다.
    - 스프링이 관리하는 객체에서만 동작. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.

- @Component : 이 애노테이션이 있으면 스프링 빈으로 자동 등록된다.
  컴포넌트를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록된다. 
  @Component > @Controller, @Service, @Repository
  기본적으로 하나씩 등록하는 싱글톤으로 등록 
  

- @SpringBootTest : 스프링 컨테이너와 테스트를 함께 실행한다.

- @Transactional : __테스트 케이스__에 이 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고,
  테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지
  않는다. 운영일땐 데이터 베이스에 적용된다.
  ( 테스트 코드에선 테스트 케이스 각각 적용. 트랜잭션 - 테스트 - 롤백 )
    - 데이터 저장,변경 할경우 트랜잭션 필요
    - [사용 주의 참고 글](https://mommoo.tistory.com/92)

- @Commit : 실행후 데이터 커밋 (테스트 실행시에도)

- 컨트롤러에서 @ResponseBody 를 사용하고, 객체를 반환하면 객체가 JSON으로 변환

- @GeneratedValue : DB가 자동으로 id값 생성
```java
@Id @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id; // 시스템 ID
```

- @Aspect : [AOP](./AOP.md) 에서 사용
- Advice 5종류 
  - @BeforeEach : Target 메서드 호출 이전에 적용. 테스트가 서로 영향이 없도록 항상 새로운 객체를 생성하고, 의존관계도 새로 맺어준다.  ( = @Before)
  - @AfterRunning : Target 메서드가 성공적으로 실행되고, 결과값 리턴한 뒤 적용
  - @AfterThrowing : Target 메서드에서 예외 발생 이후 적용(try/catch의 catch)
  - @AfterEach : Target 메서드에서 예외 발생에 관계없이 적용 ( = @After)
  - @Around : Target 메서드 호출 이전과 이후 모두 적용(메서드의 호출 자체를 제어할 수 있어 가장 강력)
  
- - - 
- [트랜잭션](https://devuna.tistory.com/30)