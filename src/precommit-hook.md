# Precommit hook

Precommit hook을 사용하면 커밋하기 전에 테스트를 실행해서, 테스트가
실패하면 커밋이 만들어지지 못하게 만들 수 있다. 실수로 이상한 커밋을
만들지 못하게 안전장치 역할을 한다.

## 설치

[husky
가이드](https://typicode.github.io/husky/#/?id=automatic-recommended)
따라서 설치하자.

## pre-commit 훅 설정

설치가 완료되면 .husky 디렉토리에 pre-commit 파일이 있다. 이 파일에서
테스트 코드를 실행하면 된다. 우리는 Gradle로 프로젝트를 관리하기
때문에 gradle을 사용해서 테스트를 실행하자.

프로젝트 루트에 있는 gradlw에 test 인자를 주고 실행하자.

```
../gradlew test
```
