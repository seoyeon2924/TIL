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

객체를 JSON으로 바꿔주는 유명한 라이브러리 2개 : JACKSON, GSON
