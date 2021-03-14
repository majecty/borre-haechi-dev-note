# Aggregate

## 마틴 파울러가 쓴 DDD_Aggregate에 관한 글에 따르면

링크: <https://martinfowler.com/bliki/DDD_Aggregate.html>

DDD aggregate는 하나의 단위로 관리할 수 있는 여러 도메인 오브젝트의
모음이다. 밖에서 aggregate 안의 값을 접근하려면 항상 aggregate root에
접근해야 한다.

"트랜잭션은 aggregate 경계를 넘어선 안된다."는 아직 이해하지 못했다.

aggregate를 도입하는 이유는, DDD 방식이 일반적인 RDBMS 디자인에 비해서
작은 오브젝트와 테이블을 많이 만들기 때문으로 보인다. 이를 하나의
단위로 묶어서 관리 비용을 줄이는 것이다.

## Domain-Driven Design

6.3 Aggregate invariants

Aggregate 경계 바깥에서는 Aggregate 안의 객체에 대한 레퍼런스를
가지지 않는다. 항상 Aggregate root를 통해서 접근해야 Aggregate의
integrity를 보장할 수 있기 때문이다.

Aggregate 경계 안에서 커밋을 할 때에는 invariant가 전부 만족되어야
한다.

## JPA Cascade를 사용해서 Aggregate 구현하기

JPA의 Cascade기능을 써서 Aggregate를 구현할 수 있다.
