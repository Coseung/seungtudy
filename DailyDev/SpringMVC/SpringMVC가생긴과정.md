
# 정적 웹페이지

초창기 웹 애플리케이션은 **정적인 웹페이지만 제공**했다.  
그냥 서버에 있는 HTML 파일 그대로 내려줬고 사용자마다 달라지는 **동적 컨텐츠**를 만들기가 어려웠음.
![](https://velog.velcdn.com/images/dreamdp01/post/bdfdc66b-5148-4577-aa9f-9142035bdc47/image.png)


---

## 동적 컨텐츠 제공을 위해 CGI 등장

그래서 동적 컨텐츠를 제공하려고 **CGI(Common Gateway Interface)** 가 나왔다.  
CGI는 웹 서버가 요청을 받으면, 그 요청을 처리할 프로그램을 실행해서 **동적인 데이터를 만들어 응답**해주는 방식이다.

근데 동적은 됐는데 문제점이 있었다.
![](https://velog.velcdn.com/images/dreamdp01/post/d7067c97-4232-41e9-a048-e383b5e022f5/image.png)


### CGI 문제점
- **모든 사용자 요청마다 프로세스를 새로 만들어서 처리**함  
- 요청 올 때마다 프로세스 생성 → 작업 끝나면 종료  
- 즉, 매번 객체 생성하고 종료하는 흐름이 반복됨  
→ 이게 결국 **불필요한 리소스 낭비**로 이어지고, 트래픽 많아지면 성능이 확 떨어진다.
![](https://velog.velcdn.com/images/dreamdp01/post/2f3c1609-0e42-42f2-83d2-c958c95828b3/image.png)


---

## CGI의 문제를 해결하려고 Servlet 등장
![](https://velog.velcdn.com/images/dreamdp01/post/5f3d9569-5c8e-4cba-88cd-b419b9f3ccf4/image.png)

CGI의 요청마다 프로세스을 생성 하는 문제를 해결하려고 **Servlet**이 나왔다.

Servlet은 핵심은

- 프로세스를 계속 새로 만들지 말고
- **하나의 프로세스에서 요청을 스레드로 처리하자**

즉, **프로세스 기반 → 스레드 기반**으로 바꼈다고 볼수있다.

그리고 서블릿 구현체도 보통 **한 번 만들어두고 재사용**하는 구조라(싱글톤처럼 동작)  
매번 객체 만들고 부수는 낭비를 줄여서 성능이 훨씬 좋아졌다.

---

## Servlet은 누가 실행시키냐?

Servlet은 개발자가 직접 new 해서 실행하는게 아니라,  
**Servlet Container**가 수행한다.

컨테이너가 하는 일은

- 서블릿 생성/관리
- 요청 올 때마다 스레드 할당해서 서블릿 호출
- 생명주기도 관리

서블릿 생명주기는 대표적으로:
- 생성 시 `init()`
- 제거 시 `destroy()`

---

## 근데 Servlet의 문제

요청이 늘어나면 보통 이런 식으로 URL이 많아진다.

- `/item`
- `/product`
- `/myPage`
- ...

이걸 전부 **서블릿 하나씩 만들어서 처리**하면,
서블릿이 계속 늘어나고 문제는 더 커진다.

### Servlet 방식의 불편한 점
- 각 서블릿마다 공통으로 들어가는 코드가 계속 생김  
  (예: 인증, 로깅, 인코딩 처리, 예외 처리 등)
- 즉, **중복 코드가 너무 많아지고 유지보수가 힘들어짐**

---

## 그래서 나온게 Front Controller Pattern
![](https://velog.velcdn.com/images/dreamdp01/post/0aa42ddf-0e26-4689-b35f-57aa49807625/image.png)

이 문제를 해결하려고 나온 아이디어가:

“최앞단에 컨트롤러를 하나 두고, 모든 요청을 일단 거기로 몰아넣자”

이게 **Front Controller Pattern**이다.

구조는 이런 느낌이다.

1. 모든 요청은 Front Controller가 먼저 받음  
2. 요청 URL/조건 보고
3. 해당 요청을 처리할 실행 객체(컨트롤러/핸들러)를 찾아서 위임

이렇게 하면:
- 공통 로직을 Front Controller 한 곳에서 처리 가능
- 요청별 로직만 각 컨트롤러에 분리 가능
- 유지보수가 훨씬 쉬워진다

---

## 이런 흐름을 바탕으로 Spring Web MVC 등장
![](https://velog.velcdn.com/images/dreamdp01/post/9a261039-df1d-48cb-b6b0-2653a1ebe252/image.png)


이 과정을 바탕으로 **Spring Web MVC**가 나타난다.

Spring MVC는 Front Controller를 프레임워크 레벨에서 표준으로 만든거고,
여기서 Front Controller 역할을 하는 게 **DispatcherServlet**이다.

즉,

- Front Controller → **DispatcherServlet**
- 모든 요청은 DispatcherServlet로 들어감
- DispatcherServlet이 요청에 맞는 컨트롤러를 찾아 실행시켜줌

---

## Spring MVC 
공식 문서 
![](https://velog.velcdn.com/images/dreamdp01/post/292fb02f-8ce0-4a5f-9271-3419d480dd26/image.png)

Spring Web MVC는 서블릿 API를 기반으로 구축된 최초의 웹 프레임워크이며, Spring Framework에 처음부터 포함되어 있습니다. 정식 명칭인 "Spring Web MVC"는 소스 모듈의 이름에서 유래했지만 spring-webmvc, 일반적으로 "Spring MVC"로 더 잘 알려져 있습니다.


#### spring MVC 패턴의 실행 흐름은 다음에 정리 해놓았다.
[Spring MVC 실행 흐름](https://velog.io/@dreamdp01/Spring-MVC-%EC%8B%A4%ED%96%89-%ED%9D%90%EB%A6%84-%EC%A0%95%EB%A6%AC)

---

## 한 줄 결론

정적 웹 → CGI(동적 가능 but 프로세스 낭비) → Servlet(스레드/재사용으로 성능 개선)  
근데 서블릿 많아지면 중복 지옥 → Front Controller로 공통 처리 통합  
그리고 그 Front Controller를 프레임워크로 정리한 게 **Spring MVC(DispatcherServlet)** 이다.
