# Bounded Wildcards

<https://docs.oracle.com/javase/tutorial/extra/generics/wildcards.html>에서
Bounded Wildcards 항목 참조

`Collection<?>`는 모든 `Collection<T>` 타입의 super 타입이다.

`Collection<T extends Shape>`는 `Shape`를 상속한 모든 타입 `X`에
대해서, `Collection<X>`의 super 타입이다.

일반적으로 `Collection<Child>`는 `Collection<Parent>`의 자식 타입이
아니다. 따라서 Bounded Wildcard로 Collection의 상속 관계를 쉽게 표현할
수 있다.

## 왜

왜 `Collection<Child>`를 `Collection<Parent>` 대신 쓸 수 없는 걸까. 그
이유는 변경 가능성 때문이다. 단적으로 `add`를 쓸 수 없다.
`Collection<Child>` 타입의 값을 `Collection<Parent>`타입으로 읽는
상황을 가정하자. `Collection<Parent>`에는 `OtherChild`타입의 값을
추가할 수 있지만 `Collection<Child>`에는 불가능하다. 따라서
`Collection<Child>`를 `Collection<Parent>` 대신 쓸 수 없다.

Wildcard를 사용하면 wildcard에 해당하는 타입을 만들 수 없기 때문에
안전하게 subtype과계를 만들 수 있다.
