#스프링 DB 접근 기술

H2 DB

- 어드민 화면 제공
- 교육용으로 좋음
- 용량 작음

---

#통합테스트 방법
IntegrationTest를 만들기

- @SpringBootTest : 스프링 컨테이너와 함께 테스트 수행
- @Transactional : 테스트 완료후에 항상 롤백
- @BeforeEach 랑 @AfterEach 삭제하고 @Autowired로 필드기반으로 쓰기

####@AfterEach가 필요없는 이유 ?
=> @Transactional이 있기 때문!
테스트 케이스에 이 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.

단위테스트는 JAVA CODE로 테스트!
통합테스트는 @SpringBootTest !

---

#스프링 JdbcTemplete!
@SpringJdbcTemplete
->실무에서도 많이씀

Control + R -> 마지막 실행을 한번더 실행해줌

---

#JPA

객체중심의 설계로 패러다임 전환을 가능하게 함
개발 생산성을 크게 높임

**JPA란 인터페이스이고 hibernate는 구현체! 거의 하이버네이트만 쓴다!**
JPA 는 ORM기술표준 => 오브젝트와 릴레이션테이블을 매핑! 해주는 기술 표준이라는 뜻.

@Entity => JPA가 관리하는 객체 라는 뜻!
@GenerationedValue

EntityManager em 을 이용해서 save, findById, findByName 등등을 할 수 있다.
JPQL -> 객체를 대상으로 날리는 쿼리! entity manager로 쿼리를 날리면 알아서ㅂ
