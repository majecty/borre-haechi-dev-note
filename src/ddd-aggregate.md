# Aggregate

## Domain-driven design 책에서

6장 "The life cycle of a domain object"에 Aggregates에 대한 내용이
정리되어 있다.

Aggregates는 도메인 객체 사이의 소유 관계를 사용해서 도메인 모델의
복잡성을 줄이는 방법이다.

DDD 책의 예시를 가져오겠다, `자동차`, `타이어`, `바퀴 휠` 모델이 있다.
`타이어`와 `바퀴 휠`, `자동차` 모델은 서로 함께 수정되어야 할 때가
많다. 실수로 하나의 모델만 수정하게 되면 쉽게 버그가 만들어진다.

이를 단순화하는 방법으로 Aggregates를 쓸 수 있다. `타이어`나
`바퀴휠`을 수정하는 코드를 항상 `자동차`를 통해서 호출하게 만드는
것이다. `자동차`는 필요한 맥락을 전부 알고 있으므로 실수할 여지가
적다. `자동차`를 사용하는 코드도 디테일에 신경쓰지 않을 수 있어서 코드
작성이 쉬워진다.

## Aggregates를 JPA에서 사용하기

JPA의 Cascade 옵션을 사용하면 편라하게 Aggregate 구조를 사용할 수
있다. Cascade 옵션으로 루트 객체를 저장할 때 자식 객체까지 다
저장하거나, 루트를 지울 때 자식 객체까지 전부 지워지게 만들 수 있다.

## 마틴 파울러가 쓴 DDD_Aggregate에 관한 글

링크: <https://martinfowler.com/bliki/DDD_Aggregate.html>

짧게 Aggregate가 무엇이고, 어떻게 쓸 것인지 잘 정리되어 있다.
