# Spring Boot Web Filter를 사용해서 request log 남기기

[스택오버플로우
답변](https://stackoverflow.com/questions/33744875/spring-boot-how-to-log-all-requests-and-responses-with-exceptions-in-single-pl/43155103#43155103)에서
리퀘스트 로그를 남기는 방법을 찾았다.

## Web Filters

[Spring Boot Features
페이지](https://docs.spring.io/spring-boot/docs/2.4.3/reference/html/spring-boot-features.html#boot-features-webflux-web-filters)에
Web Filters에 대한 내용이 짧게 적혀 있다.

Spring Boot WebFlux[^webflux]를 사용할 때 Web Filter를 사용하면
핸들러가 request를 처리하기 전 request를 수정할 수 있다. 핸들러가
request를 처리한 후 response를 수정할 수 있다. request나 response의
로그를 남기는 것도 Web Filter를 사용할 수 있다.

Web Filter를 Bean으로 Spring DI에 등록하면 Spring Boot Application이
자동으로 불러와서 사용한다. Spring DI에 대한 내용은 이 사이트의
[Spring DI](./spring-di.md) 글에 정리되어 있다.

*spring-web.jar*에 제공되는 filter들이 있다. 이들을 잘 설명한 공식
문서는 찾지 못했고, [spring-web의
JavaDoc페이지](https://www.javadoc.io/doc/org.springframework/spring-web/latest/org/springframework/web/filter/package-summary.html)에서
간단한 설명을 찾을 수 있다.

[^webflux]WebFlux가 무엇인지 모르겠지만, spring boot tutorial을 따라했다면
WebFlux를 쓰고 있을 것이다. Spring Boot Web application이 제공하는
인터페이스가 여럿 있고, 가장 최신 방법이 WebFlux인 것으로 보인다.

