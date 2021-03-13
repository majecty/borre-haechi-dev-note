# Null

Java는 모든 reference 타입이 null을 가질 수 있기 때문에 조심해야 한다.

## @NonNull

다양한 패키지에서 NonNull 어트리뷰트를 제공한다. 해치랩스에서는 어떤
방식을 쓰고 무엇이 어떤 효과를 가지는지 알아보자.

### IntelliJ @Nullable과 @NotNull

[IntelliJ IDEA의 @Nullable과 @NotNull에 대한 블로그](https://www.jetbrains.com/help/idea/nullable-and-notnull-annotations.html)를 참고하자.

이 어노테이션을 사용하면 IntelliJ IDEA가 NullPointerException이 나올
수 있는 경우에 경고를 한다.
