# Spring DI

스프링 라이브러리의 핵심 기능 중 하나가 의존성 주입이다. 의존성
주입이란, 객체 A가 객체 B를 필요로 할 때, 객체 A에게 객체 B를 넣어주는
것이다. 반대로, 객체 A가 객체 B를 필요로 할 때 객체 A가 객체 B를
생성하는 것은 의존성 주입이 아니다.

의존성 주입을 하면 기능을 사용하는 코드의 변경 없이 기능을 제공하는
코드의 구현을 쉽게 바꿀 수 있다.

## 우리는 언제 쓰는가

설정 값들과 싱글턴 객체들(Factory가 많다.)을 만들기 위해서 사용한다.
웹 서비스 레이어나, 도메인 레이어에서 필요한 객체들을 전달받는다.

예를 들어 비트코인 노드의 주소나, 이더리움 노드에 요청을 보낼 수 있는
클라이언트 등을 의존성 주입으로 처리한다. 덕분에 테스트 환경에서 해당
값을 쉽게 바꿀 수 있다.

## 용어들

IoC Container는 의존성에 대한 정보를 담는 객체이다. 의존성들은 이름과
값을 가진다. IoC Container는 의존성의 이름으로 값을 찾거나 만들 수 있다.

Bean은 의존성으로 주입될 수 있는 겂을 말한다. Bean 다른 Bean을 의존할
수 있다.

## 우리는 어떻게 쓰는가

Spring Boot를 사용하기 때문에 IoC Container를 직접 만들지 않는다.
Spring Boot를 사용하면 main 함수가 정의된 class 위에
`@SpringBootApplication`을 사용한다. @SpringBootApplication을 사용하면
자동으로 IoC Container를 만들고 package를 검색해서 bean 관련 설정을
찾아낸다.[^spring-boot-application]

[^spring-boot-application]: [@SpringBootApplication 레퍼런스](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-using-springbootapplication-annotation)

Spring Boot의 웹 서비스 객체를 만들면 해당 객체는 필요한 Bean을
주입받을 수 있다. 웹 서비스 객체부터 시작해서 웹 서비스 객체가
의존하는 Bean, 그 Bean이 의존하는 Bean, ... 등이 연쇄적으로 자동생성
된다.

## Dependency 설정

Spring은 다양한 방법으로 Bean을 만들고 연결하는 방법을 표현할 수 있다.
XML을 사용하거나, Java annotation, 혹은 Java 코드를 사용해서 Bean의
연결 방법을 표시한다. 해치랩스에서는 Java annotation과 Java 코드를
통해 연결하는 방법을 사용한다.

## Bean의 이름

우리는 대부분의 경우 싱글턴 오브젝트에 대해서만 Bean을 만들기 때문에
이름을 정하지 않는다. 이름을 정하지 않으면 Spring이 적당히 이름을
만들지만 우리는 그 이름을 사용하지 않는다.

이름을 사용하지 않아도 타입을 기반으로 하는 Autowired 기능이 있기
때문에 문제 앖다.

설정값인 경우에는 BigInteger나 String 타입의 값으로 Bean을 만들기도 한다.
이 때는 `@Value` 어노테이션을 사용해서 이름을 명시한다.

## Bean 등록하기

### @Component

class 위에 `@Component`를 달면 해당 클래스는 다른 코드에서 주입받을 수
있는 Bean이 된다.

### @Service, @Controller, @Repository

`@Component` 이외에도 `@Service`나 `@Controller`, `@Repository`와 같은
어노테이션을 클래스 위에 붙이면 클래스를 Bean으로 등록할 수 있다.

한 클래스에 `@Service`, `@Controller`, `@Repository`를 사용하면 그
클래스가 스프링 디펜던시 인젝션으로 사용되는 것 뿐만 아니라 더 많은
의미 혹은 기능을 가지게 된다. 상황에 맞는 어노테이션을 사용하자.

## Bean 주입받기

### 필드에 @Autowired를 달기

IoC Container에 의해서 생성되는 객체가 @Autowired 어노테이션을 가지고
있다면, 해당 변수는 IoC Container가 자동으로 넣어준다. 우리는 Bean의
이름을 붙이지 않으므로 타입을 기반으로 값이 세팅된다.

### 생성자의 인자로 주입받기

별다른 어노테이션을 붙이지 않더라도, 생성자의 인자로 있는 값들을
주입받을 수 있다. 우리는 Bean에 이름을 붙이지 않으므로, 타입을
기반으로 값이 세팅된다.

생성자가 하나만 있으면 그 생성자가 불린다. 여러 생성자가 있으면 원하는
생성자에 @Autowired를 붙여 주어야 한다.[^constructor-one-or-autowired]

[^constructor-one-or-autowired]: <https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-spring-beans-and-dependency-injection>

## @Value

TBD

## @Configuration

TBD

## Annotation을 썼을 때 실제로 일어나는 일

TBD

## Spring Boot 없이 쓴다면

TBD

## IntelliJ 꿀팁

의존성을 받을 수 있는 경우 의존성이 적힌 코드 왼쪽에 특별한 아이콘을
표시해준다. 실수로 의존성을 찾을 수 없는 경우에 쉽게 디버깅할 수 있다.

## 레퍼런스

<https://docs.spring.io/spring-framework/docs/current/reference/html/core.html>
