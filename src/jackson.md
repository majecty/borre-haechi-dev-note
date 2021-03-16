# Jackson

자바에서 자주 사용하는 serialization 라이브러리.
[깃헙 링크](https://github.com/FasterXML/jackson)

## Field 하나인 POJO가 @RequestBody에 있을 때 에러

Spring Boot Web Application을 만들 때의 이야기다. JSON 바디를 받는
Post 요청을 처리할 때 예상치 못한 에러를 만났다. Spring Boot에서는
`@RequestBody` 어노테이션을 달아서 Body를 Deserialize한 객체를 받을 수
있다.

다음과 같이 인자 하나를 받는 API였는데, request body용 객체를
Deserialize할 때 에러가 발생했다. 해당 객체에 사용하지 않는 필드를
추가하면 잘 동작한다.

```
{
  "passphrase": "xx"
}
```

### 추측

field가 하나인 class를 Deserialize할 때 특별한 처리가 있는 것 같다. 이
동작이 설명된 곳, 혹은 코드를 찾자.
