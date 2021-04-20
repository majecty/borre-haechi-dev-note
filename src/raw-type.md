# Raw Type

<https://docs.oracle.com/javase/tutorial/java/generics/rawTypes.html>

Java의 Generic 문법이 도입되기 전 모든 객체를 `Object` 클래스로
캐스팅하여 코드 중복을 제거했다. 이 레거시 코드를 지원하기 위해서
raw타입을 사용한다. Generic 클래스 혹은 인터페이스에서 `<T>` 파트를 뺀
타입을 raw 타입이라고 부른다. `<T>` 대신 `Object`타입을 사용한 코드를
쓸 수 있다.

일반적으로 레거시 코드가 아닌 이상 `<T>`를 쓸 필요가 없다.
