# Javadoc

Javadoc은 자바로 작성한 프로그램을 문서화하는 표준 도구다.

## 자바독 생성

인텔리제이에서 함수, 클래스, 필드 이름에 커서를 두고 alt-enter를
누르면 자바독을 생성하는 메뉴 버튼이 뜬다.

## 다른 소스코드에 대한 링크

문서화할 때 다른 클래스, 변수, 필드에 링크를 걸 수 있다.

태그들에 대해 더 자세히 알고싶다면, [자주 사용하는 자바독 태그를
소개하는 블로그](https://idratherbewriting.com/java-javadoc-tags/)를
참고하자.

### @see

다른 문서를 참고해야 할 때 @see 태그를 사용해서 다른 클래스나 필드등을
링크 걸 수 있다. 단순 링크에 비해서 참고사항이라는 컨텍스트를 더
전달할 수 있다.

### @link

{@link} 를 쓰면 링크를 만들어 준다. 바로 클래스 이름을 작성하기
시작하면, 인텔리제이가 클래스 이름을 자동완성 해준다. 필드나 메쏘드를 링크걸고 싶다면 `#`으로 시작하자.

## 패키지에 대한 문서화

package-info.java 함수를 만든 뒤 해당 파일의 패키지 선언 위에서 작성한다.

[package-info.java에 대한 스택오버플로우 답변](https://stackoverflow.com/a/624474)을 참고했다.

## private 변수에 대한 문서화

private 변수에만 자바독을 작성하고 getter와 setter에는 자바독을
작성하지 않으면 해당 자바독은 코드를 직접 읽지 않는 이상 보이지
않는다. 자바독을 웹페이지로 렌더하거나, IDE에서 문서를 보여줄 때
보이지 않게 된다.

Klaytn의 Caver-java도 해당 문제를 가지고 있다. 관련된 [이슈](https://github.com/klaytn/caver-java/issues/208)가 등록되어 있다.

lombok의 @Getter와 @Setter 어트리뷰트를 사용해서 getter와 setter를
생성한 경우 private 변수에 적은 자바독을 getter와 setter에 복사해준다.[^javadoc-getter-setter-lombok]

[^javadoc-getter-setter-lombok]: 이 [링크](https://projectlombok.org/features/GetterSetter)를 참고하자. 완전히 복사는 아니고 좀 더 복잡한 설정이 가능하다.
