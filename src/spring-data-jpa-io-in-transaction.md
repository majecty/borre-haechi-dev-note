# (TBD) Spring Data JPA Transaction중 IO

Spring Data JPA를 쓸 때 서비스 단위에서 Transaction을 건다. Service
안에서 비트코인, 이더리움, 클레이튼 노드에 요청을 할 때가 꼭 생긴다.
이 때 트랜잭션 안에서 IO를 하는 건 디비 성능에 문제를 일으킬 수 있다.

## 베스트 프랙티스를 찾아보자
