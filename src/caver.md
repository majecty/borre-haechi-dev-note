# Caver

## block에 담겨있는 트랜잭션 타입

`Transaction.TransactionData` 이다. 문서에는 트랜잭션 타입이라는 말만
있고. 정확한 클래스 이름을 알려주지 않는다.

## ValueTransferTransaction의 value 조심

문자열로 넣으면 해당 문자열이 hexa decimal format으로 인식한다.
BigInteger 타입으로 넣으면 원하는 겂이 잘 들어간다.
