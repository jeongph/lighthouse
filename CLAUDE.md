# Lighthouse (home)

## 개요
이 파일은 Claude Code (claude.ai/code)가 Jeongph 관련 작업할 때 참고하는 최상위 지침이다. 따라서, Claude 실행 위치가 현재 이곳 `./lighthouse` 가 아니라면, 사용자에게 위치를 재확인한다.
이 디렉토리는 개인 개발 루트로, 컨벤션 문서와 여러 프로젝트를 포함하는 워크스페이스이다.

## 설계 원칙
- **유연하고 확장성 있는 설계를 우선한다.** 당장의 편의보다 변경에 강한 구조를 선택한다.

## 최상위 정책
- 이 워크스페이스(lighthouse)의 git은 메타 파일과 컨벤션 문서만 관리한다 (`.gitignore`, `CLAUDE.md`, `README.md`, `.claude/`, `conventions/`)
- `.gitignore`에서 `*`로 전체 무시 후 관리 대상만 선택적 허용하는 구조이다
- `.gitignore`는 사용자가 명시적으로 수정을 요청하지 않는 한 절대 변경하지 않는다
- 하위 프로젝트 파일을 워크스페이스 git에 포함시키려는 시도(`.gitignore` 수정, `git add` 등)를 하지 않는다
- **최상위 정책문서(`CLAUDE.md`)가 변경되면 즉시 커밋 & 푸시한다**
- 하위 프로젝트 커밋/푸시 절차:
  1. 해당 프로젝트 디렉토리에서 `git remote -v`로 remote 확인
  2. 해당 저장소에서 개별적으로 커밋/푸시

## 커뮤니케이션 언어
- 커밋 메시지, PR, 코드 리뷰, 주석 등 모든 커뮤니케이션은 한글로 작성
- 코드(변수명, 함수명 등)는 영어

## 주석 컨벤션
- 형식: `TAG: yyyy-MM-dd 내용`
- TODO: 해야 할 작업, 아직 구현하지 않은 기능
- FIXME: 알려진 버그 또는 잘못된 동작
- XXX: 더 생각해봐야 할 부분, 비효율적이거나 의문이 드는 코드

## 커밋 가이드라인
- 형식: `<type>: <subject>`
- 제목 50자 이내, 마침표 금지
- 타입 종류: feat, fix, refactor, style, docs, test, chore
- 상세 규칙은 [conventions/git-commit-convention.md](conventions/git-commit-convention.md) 참조

## 워크플로우
- 브랜치 전략: Git Flow (main, develop, feature/<기능명>, hotfix/<이슈명>)
- Merge 정책: 사용자가 직접 확인하겠다고 하지 않는 한 PR 생성 후 바로 Merge

## 작업 히스토리
- 각 프로젝트의 `docs/history/` 디렉토리에 작업 기록
- 파일명: `yyyy-MM-dd-<author>-<작업명>.md`
- author: 작성자 구분 (claude, jeonguk 등)
- **작업 완료 시 반드시 히스토리 파일을 작성하고 커밋한다**

## 레포지토리 목록
- `infra-gitops/` — K8s GitOps 저장소, ArgoCD 기반 app-of-apps 패턴으로 배포 매니페스트 관리 (수동 명령어 사용 지양)
- `handy/` — 개발 관련 치트시트, 가이드, 팁 모음
- `helpful-feeds/` — 개발자를 위한 유용한 피드 모음
