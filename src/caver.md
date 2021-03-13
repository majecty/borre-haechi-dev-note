# Caver

## Caver-java 이해하기

Caver-java는 Klaytn 노드가 제공하는 JSON-RPC를 자바 코드로 호출할 수
있게 해준다. 안전하고 정리가 잘 된 자바 객체를 만들어주지 않기 때문에
조심해야 한다. JSON-RPC에서 응답으로 주는 JSON 값을 기계적으로 Java
Class로 매핑한 것에 가깝다. 라이브러리의 문서화보다 JSON-RPC 스펙을
참고하는 것이 더 안전하다.

이 [링크](https://docs.klaytn.com/bapp/json-rpc/api-references)에서 클레이튼의 JSON RPC 스펙을 읽을 수 있다.

## Caver-java Javadoc

Caver-java는 Javadoc 문서화가 잘 안되어 있다. 간혹 getter, setter에는
문서화가 안되어 있더라도, private 필드에는 되어 있는 경우가 있다.
필드에 대한 설명이 궁금하면 함수의 정의로 간 뒤 소스코드를 읽어보자.

## block에 담겨있는 트랜잭션 타입

`Transaction.TransactionData` 이다. 문서에는 트랜잭션 타입이라는 말만
있고. 정확한 클래스 이름을 알려주지 않는다.

## ValueTransferTransaction의 value 조심

문자열로 넣으면 해당 문자열이 hexa decimal format으로 인식한다.
BigInteger 타입으로 넣으면 원하는 겂이 잘 들어간다.
