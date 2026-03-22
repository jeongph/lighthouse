# Git commit convention - 예시 및 참고 자료

> 규칙 원본: [conventions/git-commit-convention.md](../conventions/git-commit-convention.md)

## 예시

- 공식 문서 예시

```
feat($compile): simplify isolate scope bindings

Changed the isolate scope binding options to:
  - @attr - attribute binding (including interpolation)
  - =model - by-directional model binding
  - &expr - expression execution binding

This change simplifies the terminology as well as
number of choices available to the developer. It
also supports local name aliasing from the parent.

BREAKING CHANGE: isolate scope bindings definition has changed and
the inject option for the directive controller injection was removed.

To migrate the code follow the example below:

Before:

scope: {
  myAttr: 'attribute',
  myBind: 'bind',
  myExpression: 'expression',
  myEval: 'evaluate',
  myAccessor: 'accessor'
}

After:

scope: {
  myAttr: '@',
  myBind: '@',
  myExpression: '&',
  // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
  myAccessor: '=' // in directive's template change myAccessor() to myAccessor
}

The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

- 유저 회원가입 기능을 만들었다면

```
feat: 유저 회원가입 기능 구현

- OAuth 를 사용한 유저 회원가입 기능 추가
- 회원 이메일 중복확인을 위한 함수 추가

See EAB-10
Closes #123, #101
```

- 버그 수정을 했다면

```
fix: 유저 회원가입 버그 수정

- 회원가입 시 모든 유저의 핸드폰 번호가 010-0000-0000 으로 수정되던 버그 수정

See EAB-12
Close #143
```

- 문서를 작성해서 추가했다면

```
docs: `Users` table Create SQL 추가
```

## 추가로 참고할만한 것들

- [Gitmoji 사용하기](https://treasurebear.tistory.com/70)
- [좋은 git commit 메시지를 위한 영어 사전](https://blog.ull.im/engineering/2019/03/10/logs-on-git.html)
- [GitHub 변경 사항을 자동으로 log로 만들고 release 하기](https://musma.github.io/2020/07/27/changelog.html)

## References

- [\[Git\] 커밋 메시지 규약 정리 (the AngularJS commit conventions)](https://velog.io/@outstandingboy/Git-%EC%BB%A4%EB%B0%8B-%EB%A9%94%EC%8B%9C%EC%A7%80-%EA%B7%9C%EC%95%BD-%EC%A0%95%EB%A6%AC-the-AngularJS-commit-conventions)
- [좋은 git 커밋 메시지를 작성하기 위한 7가지 약속](https://meetup.nhncloud.com/posts/106)
- [Conventional Commits](https://www.conventionalcommits.org/ko/v1.0.0/)
- [git: AngularJS Git Commit Message Conventions](http://dogfeet.github.io/articles/2013/angularjs-git-commit-message-conventions.html)
- [How to Write a Git Commit Message](https://cbea.ms/git-commit/)
- [Linking a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)
- [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional)
- [Commit Message Guidelines](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)
- [\[우아한테크코스/프리코스\] Commit Message Conventions](https://programmer-ririhan.tistory.com/335)
