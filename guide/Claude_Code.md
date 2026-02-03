# Claude Code 가이드

## CLAUDE.md 정책 파일 탐색 범위

Claude Code는 **현재 작업 디렉토리(cwd)에서 시작하여 루트 디렉토리(`/`) 바로 전까지 재귀적으로 올라가며** 모든 `CLAUDE.md` 및 `CLAUDE.local.md` 파일을 탐색한다.

즉, 상위 폴더 제한 없이 파일시스템 루트 직전까지 전부 참고한다.

### 탐색 순서 및 우선순위

- 상위 디렉토리일수록 먼저 로드되어 기본 정책 역할을 한다.
- 하위 디렉토리의 `CLAUDE.md`가 그 위에 덧씌워지는 구조이다.
- `~/.claude/CLAUDE.md`는 모든 세션에 전역으로 적용된다.

### 예시

`/Users/jeonguk/Dev/myrepo/packages/frontend/`에서 Claude Code를 실행하면:

```
~/.claude/CLAUDE.md                                    ← 전역 (항상 로드)
/Users/CLAUDE.md                                       ← 있으면 로드
/Users/jeonguk/CLAUDE.md                               ← 있으면 로드
/Users/jeonguk/Dev/CLAUDE.md                           ← 있으면 로드
/Users/jeonguk/Dev/myrepo/CLAUDE.md                    ← 있으면 로드
/Users/jeonguk/Dev/myrepo/packages/CLAUDE.md           ← 있으면 로드
/Users/jeonguk/Dev/myrepo/packages/frontend/CLAUDE.md  ← 있으면 로드
```

### 하위 디렉토리 CLAUDE.md

하위 디렉토리의 `CLAUDE.md`는 시작 시 로드되지 않고, 해당 디렉토리의 파일을 읽을 때 **온디맨드로 로드**된다.

### settings.json과의 차이

`settings.json`은 `CLAUDE.md`와 달리 상위 디렉토리 탐색을 **지원하지 않는다**. 현재 cwd의 `.claude/settings.json`만 참조한다.

### 전체 설정 우선순위

```
Enterprise > CLI > Local Project > Shared Project > User > Defaults
```

### `@` import 구문

- `CLAUDE.md` 파일 내에서 `@path/to/file` 구문으로 다른 파일을 import할 수 있다.
- 상대 경로는 import를 포함하는 파일 기준으로 해석된다.
- 최대 **5단계 깊이**까지 재귀 import이 가능하다.

### CLAUDE.local.md

- `CLAUDE.local.md`는 자동으로 `.gitignore`에 추가된다.
- 버전 관리에 포함하지 않을 개인 설정에 적합하다.

### 참고 자료

- [Manage Claude's memory - Claude Code Docs](https://code.claude.com/docs/en/memory)
- [Using CLAUDE.MD files](https://claude.com/blog/using-claude-md-files)
