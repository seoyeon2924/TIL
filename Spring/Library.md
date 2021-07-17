#라이브러리 살펴보기
gradle이 의존성 관리를 해주기 때문에, Library를 알아서 땡겨 준다.

---

#WAS서버

- 요즘에는 톰캣을 깔 필요가 없음. 스프링 부트가 내장 서버를 들고 있기 때문에 ..

---

#Logging 방법

- System.out.println으로 log 남기면 안되요..
- Spring-boot-starter-logging ->logback과 sl4j를 많이써서 아예 라이브러리에서 땡겨 온다!

---

#테스트 라이브러리

- 요즘은 Junit5
- mockito
- assertJ

---

#spring boot는 시작할때 알아서 index.html을 먼저 찾음.

---

#Thymeleaf 템플릿 엔진

- freeMarker

- Thymeleaf등의 템플릿 엔진을 제공함
- ㅌㅔ스트

---

#Return Hello 하면?

#Return Hello 하면?  
return hello 하면 알아서 hello.html을 찾아감.
-> 컨트롤러에서 리턴값으로 문자를 반환하면 뷰 리졸버가 화면을 찾아서 처리한다!

---
