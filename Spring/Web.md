#스프링 웹개발 기초

##정적 컨텐츠

**웹브라우저가 톰캣에 localhost:8080/hello-static.html 요청할 경우**

1. controller에 hello-static 관련 controller가 있는지 확인
2. 없으면 resources에 static 폴더에 hello-static.html이 있는지 확인 -> 있으면 브라우저에 반환(그대로 반환, HTML을 변환하지 않음)

---

##MVC와 템플릿 엔진

mvc : model, view, controller

**웹 브라우저가 localhost:8080/hello-mvc 요청한 경우**

1. controller에 hello-mvc를 찾아감
2. controller는 model에 필요한 데이타(예를 들면 request param으로 넘어온 데이터)를 넣어서 view resolve에게 hello-mvc를 반환
3. view-resolver는 hello-mvc.html을 찾아서 브라우저에 반환 (렌더링이 된html로 변환해서! )

---

##API
데이터를 내릴땐, 정적으로 내리는경우가 아니라면 2가지 고민하면 된다
->HTML로 내리냐(MVC방식)
->API로 바로 내리느냐

API로 내릴때는 @ResponseBody가 꼭 필요함 -> http 통신 프로토콜에서 body부분에 데이터를 직접 넣겠다는 뜻!

- return String이면 무식하게 문자 그대로 내려감. 템플릿 엔진 없음. String Converter
- return에 객체를 반환할 경우 ? => JSON 형태로 넘어가게 됨 (물론 xml로 변경할 수도 있으나 json을 많이 씀)

**웹브라우저가 localhost:8080/hello-api 요청한 경우**

1. controller에 hello-api를 찾아감 -> 어라? @ResponseBody가 있네?
2. ResponseBody가 있을 경우, View-resolver가 아닌 jsonConverter나 StingConverter와 같은 HttpMessageConverter에게 데이터를 전달
3. 브라우저에 문자나 json을 반환

객체를 JSON으로 바꿔주는 유명한 라이브러리 2개 : JACKSON, GSO

---

#일반적인 웹서비스 구조

컨트롤러 -> 서비스 -> 리포지토리 -> DB
도메인

DB를 바꿔끼우려면 인터페이스가 필요하다.

---

#테스트케이스 작성

JUNIT -> 테스트코드 프레임워크를 사용한다.
`org.assertj.core.api.Assertions.assertThat`을 사용하면 문법적으로 좀 더 쉽다.

테스트코드는 순서가 랜덤하게 테스트 되기 떄문에 순서에 종속적이지 않은 테스트 코드를 짜야한다
즉, 공용 공간을 깨긋이 지워줘야 한다.

-> 이를 위해 @AfterEach를 사용한다.

각 @Test가 끝날때마다 @AfterEach안의 내용이 실행 된다.

```java
@AfterEach
public void afterEach() {
  repository.clearStore();
}
```

---

#코드 리팩토링
ctrl+T : 블록으로 잡고 ctrl+T 하면 리팩토링 관련된 기능을 볼 수 있음.

- extract method -> 선택 부분을 함수로 뺄수있음

---

#테스트 코드 작성에서의 DI

MemberService의 테스트 코드 작성 시, 클리어를 위해 MemoryMemberRepository를 이용함.
MemoryMemberRepository는 근데, MemberService에서도 New하는데 MemberServiceTest에서도 New 하니까 서로 다른 Instance를 가지고 테스트 하는 격!

그렇기 떄문에 MemberRepository는 MemberService에서 생성자를 통해 주입받도록 하고,
MemberServiceTest에서도 테스트 시작전 코드를 실행하는 @BeforeEach를 통해 생성자를 통한 MemberService를 생성한다.

```java
public class MemberService {

  private final MemberRepository memberRepository ;

  // 테스트에서 쓰는 MemberRepository랑 같은 객체이고 싶어서 생성자를 통해 넣는거야.
  public MemberService(MemoryMemberRepository memoryMemberRepository) {
    this.memberRepository = memoryMemberRepository;
  }

  ...중략....
}
```

```java
class MemberServiceTest {

  MemberService memberService;
  MemoryMemberRepository memoryMemberRepository;
  // MemoryMemberRepository memoryMemberRepository = new MemoryMemberRepository();

  @BeforeEach
  public void beforeEach() {
    memoryMemberRepository = new MemoryMemberRepository();
    memberService = new MemberService(memoryMemberRepository);
  }

  @AfterEach
  public void afterEach() {
    memoryMemberRepository.clearStore();
  }

  ....중략......
}
```

---

#스프링 빈

@Controller @Service @Repository
스프링 컨테이너에서 스프링 빈이 관라된다

##스프링 빈 등록하는 두 가지 방법

1. 컴포넌트 스캔과 자동의존관계 설정 -> 스프링이 딱 뜰 때 스프링 컨테이너에 @Component를 스캔해서 빈을 등록하는 거고 그때 @Autowired가 연결을 해주는 역할.
   @Service 안에 @Component가 되어있음. 즉 서비스는 서비스 역할에 특화된 Component
   package hello.hellospring를 포함한 하위 패키지의 @Component들을 등록함.

- 참고 : 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다. 따라서 같은 스프링 빈이면 같은 인스턴스이다.
  설정으로 싱글톤이 아니게 할수 는 있지만 대부분의 경우는 싱글톤을 이용한다.

2. 자바코드로 직접 스프링 빈 등록하기  
   @Configuration을 통해서 설정하는 방법
   서비스의 @Service와 레포지토리의 @Repository 지우고 @Autowired도 삭제한 상태에서 하기와 같이 SpringConfig.class 작성
   -> @Bean을 통해 스프링빈으로 등록
   -> 서비스에 생성자에 MemberRepository를 주입함으로써 의존관계 주입 설정이 됨

```java
@Configuration
public class SpringConfig {

  @Bean
  public MemberService memberService() {
    return new MemberService( memberRepository());
  }

  @Bean
  public MemberRepository memberRepository() {
    return new MemoryMemberRepository();
  }
}
```

```
실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다.
정형화 되지 않거나 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다
```

##@Autowired 3가지
@Autowired 는 **필드주입방법이 있고 생성자 주입 방법, Setter주입**이 있는데 권장되는 사항은 생성자 주입방법
스프링 뜬 다음에는 바꿀 여지가 없기 때문! Sett는 노출때문에 좋지 않은 방법
**결론 : 생성자 주입을 권장!!**

---

#HomeController

- @GetMapping("/") 이 있으면 여기를 먼저 들어감
- 없었을 때는 알아서 index.html을 찾아감 (Spring Boot 제공)

---

#회원등록 기능
###Form
form은 인풋타입은 text, name이 서버로 넘어갈때 key가 됨!
등록을 누르면 포스트 방시그로 넘어가는데

포스트 매핑은 폼에 넣어서 전달할때 포스트, 겟은
name을 보고 알아서, 그 객체의 name을 넣어줌 ..? 스프링이 알아서 setname을 통해서 값을 넣어줌

---

#조회기능 구현
###Model
import org.springframework.ui.Model;
Model은 Controller에서 data를 담아서 view로 던질 때 사용한다.

---

#MVC 정리
###View -> Controller 호춯

1. GET
2. POST (Form에 Data를 담아서 던질 수 있음)

###Controller -> View 호출

1. Model에 Data를 담아서 view에 뿌려줌!
