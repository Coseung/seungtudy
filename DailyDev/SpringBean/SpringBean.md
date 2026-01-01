![](https://velog.velcdn.com/images/dreamdp01/post/b023fa39-8630-4efa-83aa-6e819938b6a3/image.png)

## Bean이란?

스프링에서 Bean은 **스프링 컨테이너(ApplicationContext)가 생성하고 관리하는 객체**를 말한다.  
애플리케이션 코드에서 `new`로 직접 생성해서 쓰는 객체가 아니라, 스프링 설정을 통해 **컨테이너에 등록되어 관리되는 객체**가 Bean이다.

Bean은 DI(의존성 주입)에서 주입 대상이 되며, 스프링은 Bean의 생성, 의존관계 연결, 생명주기 관리까지 담당한다.

---

## Bean은 어디에서 관리되나?

Bean은 스프링 컨테이너인 `ApplicationContext`에 등록되고 관리된다.  
스프링은 애플리케이션 시작 시점에 필요한 Bean을 생성하고, 필요한 곳에 의존성을 주입한다.

---

## Bean 등록 방법

### 1) 컴포넌트 스캔 방식

클래스에 `@Component` 계열 어노테이션을 붙이면, 스프링이 컴포넌트 스캔을 통해 Bean으로 등록한다.

~~~java
@Service
public class MemberService {
}
~~~

대표적으로 다음 어노테이션이 컴포넌트 스캔 대상이다.

- `@Component`
- `@Controller`
- `@Service`
- `@Repository`

---

### 2) 자바 설정 방식(@Configuration + @Bean)

설정 클래스에서 `@Bean` 메서드를 통해 직접 Bean을 등록할 수도 있다.

~~~java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberService();
    }
}
~~~

외부 라이브러리 객체를 Bean으로 등록하거나, 생성 과정을 명확하게 통제하고 싶을 때 자주 사용한다.

---

## Bean 생명주기

Bean은 보통 아래 흐름으로 관리된다.

1. Bean 생성  
2. 의존성 주입  
3. 초기화(필요 시)  
4. 사용  
5. 소멸(필요 시)

초기화/소멸 로직이 필요하면 다음을 사용할 수 있다.

~~~java
@PostConstruct
public void init() {}

@PreDestroy
public void destroy() {}
~~~

---

## Bean Scope(범위)

Bean의 기본 스코프는 `singleton`이다.  
즉, 스프링 컨테이너 내에서 **하나의 인스턴스가 생성되어 공유**된다.

자주 쓰는 스코프:

- `singleton`(기본값): 컨테이너당 1개 인스턴스
- `prototype`: 요청할 때마다 새 인스턴스 생성

---

## 정리

- Bean은 스프링 컨테이너가 관리하는 객체이다.
- DI에서 주입 대상이 되는 객체들이 보통 Bean이다.
- 등록 방식은 컴포넌트 스캔과 `@Bean` 등록 방식이 대표적이다.
- 기본 스코프는 `singleton`이다.
