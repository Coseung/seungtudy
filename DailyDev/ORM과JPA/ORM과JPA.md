## ORM이란?
ORM이란 **객체(Object)와 DB의 테이블을 Mapping** 시켜 RDB 테이블을 객체지향적으로 사용하게 해주는 기술이다.
ORM은 SQL을 직접 작성하지 않고 Java코드로 데이터를 조작 시킬 수 있게 도와준다.
이로인해 개발자는 오직 객체에만 집중할 수 있다.

![](https://velog.velcdn.com/images/dreamdp01/post/52e21a6b-293f-449b-b6c5-5a9159dbe726/image.png)

---
### ORM의 장단점
ORM에서 주된 장점은 **생산성 향상**과 **객체 지향적인 코드 유지**라고 생각한다.

반복적인 SQL 매핑 작업을 ORM이 수행하므로, 개발자는 **비즈니스 로직 구현에 더 집중**할 수 있다는 점이 장점이고 특정 데이터베이스에 종속되지 않아 추후 **DB 변경 시에도 유연하게 대처**할 수 있다는 점이 장점이라 생각한다.

하지만 **복잡하고 정밀한 튜닝이 필요한 쿼리 작성** 시에는 직접 SQL을 작성하는 것보다 구현이 어려울 수 있고 내부 동작 원리를 이해하지 못하면 **N+1 문제**와 같은 성능 저하가 발생할 수 있기 때문에 **ORM은 SQL을 완전히 대체하는 기술이 아니고 SQL에 대한 이해를 바탕으로 적절히 활용해야 하는 도구**라고 생각한다.

---

## JPA
![](https://velog.velcdn.com/images/dreamdp01/post/2f51f5cb-5f0b-4c72-a049-5d731cb9cfc5/image.png)

JPA는 **JAVA에서 쓰는 ORM이다**. 이말은 다른언어에서도 ORM이 있다는 뜻이다. JAVA 의 ORM이 JPA일 뿐이다

JDBC를 직접 구현하게 된면 단순코드가 반복되게 되고 로직에 Connection, SQL 두개의 로직이 들어가는 등 다른기능이 로직에 들어가게 되어 가독성도 떨어진다.
JPA를 사용하면 JPA 내부적으로 SQL쿼리를 짜주고 JDBC를 사용한다.

개발자는 좀더 객체지향 적인 생각과 비지니스 로직에 집중할 수 있다는 장점이 있다.

JPA의 구현체는 대표적으로 하이버네이트(Hibernate)가 가장많이 사용된다.

---
## Hibernate 와 Spring Data JPA
![](https://velog.velcdn.com/images/dreamdp01/post/963a73af-f399-4233-af35-9fe5cfbfc922/image.png)

#### 1. Hibernate (JPA의 구현체)
JPA는 데이터베이스를 어떻게 조작할지 정의한 **명세(Interface)**일 뿐, 실제 기능이 작동하는 코드는 아니다. 이 JPA 인터페이스를 직접 구현하여 기능을 제공하는 라이브러리가 바로 **하이버네이트(Hibernate)**다.

#### 2. Spring Data JPA (편의성을 위한 모듈)
Spring Data JPA는 개발자가 하이버네이트(JPA 구현체)를 더욱 편하게 사용할 수 있도록 스프링 프레임워크에서 제공하는 모듈이다.

**JPA/Hibernate 사용 시**: `EntityManager`를 직접 생성하고 트랜잭션을 관리하며 코드를 작성해야 한다.

**Spring Data JPA 사용 시**: 복잡한 `EntityManager`를 직접 다루지 않고, JpaRepository 인터페이스를 상속받는 것만으로도 단순한 CRUD 기능을 구현할 수 있다.

즉, Spring Data JPA는 메서드 이름만으로 SQL을 동적으로 생성해주고 복잡한 설정(Repository 설계 등)을 대신 처리해줌으로써, 개발자가 하이버네이트를 훨씬 직관적으로 사용할 수 있게 돕는 역할을 한다.

---
