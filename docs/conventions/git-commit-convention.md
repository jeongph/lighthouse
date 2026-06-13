# 커밋 컨벤션

이 문서는 [Conventional Commits 1.0.0](https://www.conventionalcommits.org/ko/v1.0.0/) 을 기반으로 한다. 표준 구조와 규칙을 따르되, 한글 작성 등 우리 팀의 규칙을 더한다.

## TL;DR

- format
``` sh
<type>(<scope>): <subject>      # (<scope>) 생략 가능 / 단절적 변경이면 : 앞에 !
<BLANK LINE>
<body>                          # 생략 가능
<BLANK LINE>
<footer>                        # 생략 가능
```

- kind of the `<type>`

| type | 설명 | SemVer |
|------|------|--------|
| `feat` | 새로운 기능을 추가했을 때 | MINOR |
| `fix` | 버그를 수정했을 때 (bug fix) | PATCH |
| `perf` | 동작 변화 없이 성능을 개선했을 때 | - |
| `refactor` | 기존 동작은 그대로 두고 코드 리팩터링을 수행했을 때 | - |
| `style` | 코드와 기능 수정 없이 코드 모양(들여쓰기, 줄맞춤, 포맷)을 수정했을 때 | - |
| `docs` | 문서를 수정했을 때 (README, SQL, Javadoc) | - |
| `test` | 테스트 관련 변경사항 | - |
| `build` | 빌드 시스템이나 외부 의존성을 변경했을 때 (gradle, npm 등) | - |
| `ci` | CI 설정이나 스크립트를 변경했을 때 (GitHub Actions 등) | - |
| `chore` | 위에 속하지 않는 잡일 (패키지 매니저 설정 등 코드 변경 없는 작업) | - |
| `revert` | 이전 커밋을 되돌릴 때 | - |

> 타입과 무관하게 **단절적 변경(BREAKING CHANGE)** 이 포함되면 **MAJOR** 이다. 아래 [BREAKING CHANGE](#breaking-change-단절적-변경) 절 참고.
> SemVer 영향은 표준이 규정하는 `feat`(MINOR) / `fix`(PATCH) / BREAKING CHANGE(MAJOR)만 명시한다. 나머지 타입은 릴리스에 직접 영향이 없으며(`-`), 분류·이력 관리 목적이다. (도구에 따라 `perf` 등을 PATCH로 올리기도 한다)

## 상세 내용

- `<type>` 과 `(<scope>)` 를 제외한 문장들은 한글 로 표기 (제목의 한글표기가 어색한경우, 영문표기 가능)
- 존대말을 사용하지 않음
- `<type>` 은 소문자로 작성하고 일관되게 유지한다 (표준은 대소문자를 구분하지 않으나 소문자로 통일). 단 `BREAKING CHANGE` 는 반드시 대문자
- `<BLANK LINE>` : 제목·본문·푸터는 각각 한 줄 띄워서 분리

### 제목 작성법

- `(<scope>)`
  - 생략 가능
  - 변경된 클래스나 함수명 혹은 그 기능을 포함하는 도메인 자체 (예시> (User), (findAll), (TeamEntity))
  - 코드베이스의 영역을 가리키는 명사로 작성
- `!` (단절적 변경 표시)
  - 호환성을 깨뜨리는 변경이면 `<type>`/`(<scope>)` 뒤, `:` 바로 앞에 `!` 를 붙인다 (예시> `feat(api)!:`)
  - 자세한 규칙은 아래 [BREAKING CHANGE](#breaking-change-단절적-변경) 절 참고
- `<subject>`
  - 한글 로 최대 50자를 넘지 않도록 작성
  - 명령문 + 현재 시제 실행문으로 작성 → 커밋 메시지는 무엇이 변경되었는지가 아니라 실제로 그 변경 사항이 미치는 영향, 즉 변경 사항이 실질적으로 무엇을 하는지 설명하기 때문 (예시> UserEntity 추가)
  - 제목에는 마침표 와 문어체 를 사용하지 않음

### 본문 작성법

- `<body>`
  - 생략 가능
  - 해당 커밋에 대한 상세 내용을 기재
  - 변경한 이유와 변경 전과의 차이점을 설명
  - 제목과 한 줄 띄우며, 자유 형식으로 여러 단락을 작성할 수 있음

### BREAKING CHANGE (단절적 변경)

호환성을 깨뜨리는 변경(API 시그니처 변경, 설정 키 제거, 응답 포맷 변경 등)은 반드시 표시한다. 어떤 타입(`feat`, `fix`, `refactor` 등)에도 포함될 수 있으며 SemVer **MAJOR** 에 해당한다.

다음 두 가지 방법으로 표시하며, 하나만 쓰거나 둘 다 쓸 수 있다.

1. `<type>`/`(<scope>)` 뒤 `:` 앞에 `!` 를 붙인다 → `feat(api)!: ...`
2. 푸터에 `BREAKING CHANGE: <설명>` 을 작성한다 (반드시 대문자, `BREAKING-CHANGE` 도 동일하게 취급)

`!` 만 사용한 경우 푸터의 `BREAKING CHANGE:` 는 생략할 수 있으며, 이때 제목이 단절적 변경의 설명이 된다.

### 푸터 작성법

- `<footer>`
  - 생략 가능
  - 본문과 한 줄 띄워서 작성
  - 각 푸터는 `토큰: 값` 또는 `토큰 #값` 형식으로 작성한다 (git trailer 형식)
  - 토큰의 공백은 `-` 로 대체한다 (예시> `Reviewed-by`, `Acked-by`). 단 `BREAKING CHANGE` 는 예외로 공백을 허용한다
  - 자주 쓰는 푸터
    - `BREAKING CHANGE: <설명>` — 단절적 변경 명시
    - `Closes #<이슈번호>` — 해결된 이슈 연결 (GitHub auto-close)
    - `Refs: #<이슈번호>` / `See <이슈키>` — 관련 이슈·외부 트래커 참조
    - `Reviewed-by: <이름>` — 리뷰어 표기
  - 푸터에 작성할 수 있는 GitHub auto-close 키워드는 [레퍼런스](#레퍼런스)의 GitHub 문서를 참조

## 예시

### 기능 추가
```
feat: 유저 회원가입 기능 구현

- OAuth 를 사용한 유저 회원가입 기능 추가
- 회원 이메일 중복확인을 위한 함수 추가

See EAB-10
Closes #123, #101
```

### 버그 수정
```
fix: 유저 회원가입 버그 수정

- 회원가입 시 모든 유저의 핸드폰 번호가 010-0000-0000 으로 수정되던 버그 수정

See EAB-12
Closes #143
```

### 문서
```
docs: `Users` table Create SQL 추가
```

### 성능 개선
```
perf: 이미지 썸네일 캐싱으로 상품 목록 응답 속도 개선
```

### CI 설정
```
ci: GitHub Actions 의존성 캐시 추가
```

### 단절적 변경 — `!` 표기
```
feat(api)!: 상품 배송 시 고객에게 이메일 발송
```

### 단절적 변경 — `!` + BREAKING CHANGE 푸터
```
refactor(auth)!: 인증 토큰 헤더를 Authorization 으로 변경

BREAKING CHANGE: X-Auth-Token 헤더는 더 이상 인식되지 않는다.
기존 클라이언트는 Authorization 헤더로 전환해야 한다.
```

### 되돌리기
```
revert: "feat: 상품 배송 시 고객에게 이메일 발송" 되돌리기

Refs: 676104e
```

## 규격 핵심 (도구 호환 체크리스트)

표준 규격 중 자동화 도구(commitlint, CHANGELOG 생성, SemVer 자동 범프) 호환에 중요한 규칙만 추렸다.

- 제목은 반드시 `<type>` + (선택)`(<scope>)` + (선택)`!` + `: `(콜론 + 공백) 으로 시작한다
- `feat` 는 기능 추가에, `fix` 는 버그 수정에만 사용한다
- 제목·본문·푸터는 각각 빈 줄 하나로 구분한다
- 단절적 변경은 `!` 또는 `BREAKING CHANGE:` 푸터로 표시한다
- `BREAKING CHANGE` 는 반드시 대문자로 작성한다
- `<type>`·`(<scope>)` 는 대소문자를 구분하지 않지만, 우리는 소문자로 통일한다

## 레퍼런스

- [Conventional Commits 1.0.0 (한글)](https://www.conventionalcommits.org/ko/v1.0.0/)
- [Conventional Commits 1.0.0 (영문)](https://www.conventionalcommits.org/en/v1.0.0/)
- [Conventional Commits 표준 GitHub 저장소](https://github.com/conventional-commits/conventionalcommits.org)
- [GitHub - Linking a pull request to an issue (auto-close 키워드)](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)
- 더 많은 예시와 참고 자료: [git-commit-convention-refs.md](../git-commit-convention-refs.md)
