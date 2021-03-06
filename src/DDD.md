# DDD

코드의 구조를 domain에 맞추는 개발 방법론이다. 서로 다른
어플리케이션이라도 같은 도메인을 사용하면 코드를 재활용할 수 있다.

## 어떻게 구현하지

블록체인 도메인에 대해서 domain 네임스페이스를 만들고 이 안에 관련
도메인을 구현한다. 최대한 primitive 타입 대신에 도메인 타입을 만들고
이 도메인 타입에 기능을 구현하는 함수를 넣는다.

## DB와의 관계

디비에 저장해야 하는 도메인 객체인 경우 JPA를 사용해서 한 도메인
객체가 테이블의 한 row가 되게 설계한다.

## 도메인과 외부 세계

도메인은 도메인 객체 이외에 의존성을 제거한다. 만약 도메인이 외부
코드를 호출해야 한다면 도메인 내부에 필요한 기능에 대해 Interface를
구현하고 이를 밖에서 구현한다. 대표적인 예시로 Bitcoin RPC client가
있다. 도메인 코드에서 Bitcoin 노드에 데이터를 요청해 받아와야 하는
경우 필요한 Bitcoin RPC 요청을 정의한 interface를 도메인에 만들고 이를
도메인 외부에서 구현한다.

## 도메인 오브젝트 만들기

### 디비 엔터티

디비에 저장되는 도메인 오브젝트를 모델링할 때 사용한다. 도메인
오브젝트는 자신이 가진 row를 외부에 노출하지
않는다.[^domain-object-get-field]

[^domain-object-get-field]: DTO를 만들기 위해선 어쩔 수 없이 노출해야
    한다. 하지만 도메인 코드 내부에서는 접근하지 말자.

### 도메인 서비스

데이터가 아닌 행위에 대한 도메인 개념들이 있다. 한 도메인 디비
엔터티만 관련이 있다면 그 도메인 오브젝트에 넣을 수 있겠지만, 여러
디비 엔터티가 관련되어 있으면 넣을 곳이 애매하다.

이를 도메인 서비스 클래스를 만들어서 해결한다. 도메인 서비스 클래스는
스테이트를 가지지 않는다. 싱글턴으로 만들며, 보통 Bean으로 만들어
사용한다.
