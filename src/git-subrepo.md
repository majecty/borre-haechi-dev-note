# Git subrepo

<https://github.com/ingydotnet/git-subrepo>

git subrepo는 super repo에서 sub repo의 이력관리를 할 수 있게 해준다.

`Common` 레포지토리를 `A`와 `B`레포가 공유한다고 가정하자. `A`는
자신만의 history로 `Common`을 수정한다. `B` 역시 자신만의 history로
`Common`을 수정한다. `A`와 `B`의 레포에서는 소스코드가 `Common`에
속하는지 안 속하는지 구분할 필요 없이 수정할 수 있다.

`A`와 `B`가 `Common`을 수정한 뒤 수정한 내역을 `Common`의 레포에
반영해주어야 한다. `git subrepo push`를 이용해 반영할 수 있으며 `git
subrepo pull`을 통해 `Common`의 변경사항을 가져올 수 있다.

subrepo의 장점은 `Common`의 변경 단위가 super repo에서 관리된다는
점이다.
