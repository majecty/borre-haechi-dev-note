# Recursive type bound

`T extends SomeInterface<T>`에 대해 알아보자.

자바의 제네릭은 제네릭 타입에 대해서 조건을 걸 수 있다. 이를 [Bounded
Type
Parameter](https://docs.oracle.com/javase/tutorial/java/generics/bounded.html)라고
부른다.

## 가장 간단한 Recursive Type Bound

비교 가능한 제네릭 타입을 받고 싶을 때 `<T extends Comparable<T>>`를
사용한다.
[`Comparable<X>`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)를
구현한 타입은 `X`와 비교할 수 있다. 따라서 `<T extends
Comparable<T>>`의 의미는 `T`는 `T`와 비교할 수 있다는 의미이다.

## 빌더

부모 클래스의 빌더를 자식 클래스에서 재활용하기 위해서 Recursive Type
Bound를 사용한다. 예를 들어 `Builder<T extends Builder<T>>`와 같은
타입을 사용한다.

자바에서는 복잡한 객체를 생성하기 위해서 빌더를 사용한다. 빌더는
체이닝되는 setter 함수로 값을 쉽게 세팅할 수 있다. 메쏘드 체이닝을
위해서는 모든 setter 함수가 자기 자신의 타입을 리턴해야 한다.

부모 클래스의 빌더 클래스를 상속한 자식의 빌더클래스를 만들 때 어려운
점이 있다. 부모 클래스의 빌더에 정의된 setter는 부모의 빌더를 리턴하기
때문에 자식 빌더에서 유용하게 쓸 수 없다. 이 때 Recursive Type Bound를
쓰면 부모 클래스의 빌더에서 정의한 함수를 자식 클래스에서 사용할 수
있다.

아래 코드의 `returnSelf`는 자기 자신인 `Parent` 타입을 반환한다.
`Parent`를 상속한 `Child`의 `returnSelf`도 `Parent`를 반환한다.

```java
class Parent {
  Parent returnSelf() {}
}

class Child extends Parent {
}

child.returnSelf() // 이 함수의 결과값은 Parent다.
```

아래 코드에서 `parent.returnSelf()`타입은 `Parent`이며
`child.returnSelf()`의 타입은 `Child`다. `returnSelf`를 `Child`에서
`override`하지 않아도 알아서 `Child`가 리턴된다.

```java
class Parent<Self extends Parent<Self>> {
  @SuppressWarnings("unchecked")
  protected Self returnSelf() {
    return (Self)this;
  }
}

class Child extends Parent<Child> {
}
```

`bitcoinj` 라이브러리의 `DetermisticKeyChain` 클래스의 빌더가 이
패턴을 사용한다. 부모 클래스인 `DeterministicKeyChain`의 빌더가
[여기](https://github.com/bitcoinj/bitcoinj/blob/68097e11f673ddaf16ec4e2f71f7e676d581f350/core/src/main/java/org/bitcoinj/wallet/DeterministicKeyChain.java#L167)에
정의되어 있다. 자식 클래스인 [`MarriedKeyChain`의
빌더](https://github.com/bitcoinj/bitcoinj/blob/68097e11f673ddaf16ec4e2f71f7e676d581f350/core/src/main/java/org/bitcoinj/wallet/MarriedKeyChain.java#L66)가
`DeterministicKeyChain`의 빌더를 상속한다.

## 왜 헷갈리는가

`<T extends Comparable<T>>`은 직관적이지 않다. 이 코드가 하고싶은 말은
"`T`는 `T` 자기 자신과 비교할 수 있다"이다. 이는 `<T is Comparable>`
이라고만 표현해도 충분하다. Java에서 인터페이스는 `Self` 타입을 쓸 수
없이 때문에 인터페이스에 직접 명시해야 한다.

`class Builder<T extends Builder<T>>` 역시 마찬가지이다. 빌더 클래스는
자기자신을 리턴하는 메쏘드를 만들고 싶지만 `Self` 타입이 없다. `T
extends Builder<T>` 파트를 단순히 `Self` 라고 이해하고, `T`가 나올
때마다 자기 자신을 의미한다고 이해하면 된다.

## 다른 언어나 환경과의 비교

다른 언어들의 상황도 비슷하다. C++에서는 [Curly recurring template
pattern](https://en.wikipedia.org/wiki/Curiously_recurring_template_pattern)이라고
부른다. C# 역시
[CRTP](https://blog.arkanosoft.com/index.php/crtp-c/)라고 부른다.

Rust는 `Self` 타입을 트레잇에서 사용할 수 있다. 다음 코드는 Rust에서
비교를 정의하는 `PartialEq`이다. `Rhs`의 기본 값으로 `Self`를
사용한다. Rust는 상속의 개념도 없기 때문에 상황이 더 간단하다.

```rust
pub trait PartialEq<Rhs: ?Sized = Self> {
    fn eq(&self, other: &Rhs) -> bool;
}
```

## 참고

<https://medium.com/@hazraarka072/fluent-builder-and-powering-it-up-with-recursive-generics-in-java-483005a85fcd>
