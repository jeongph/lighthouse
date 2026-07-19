# Git 워크플로우 컨벤션

> 이 문서는 워크스페이스 기본값이다. **프로젝트별로 다른 워크플로우를 쓸 수 있으며**, 해당 프로젝트의 `CLAUDE.md`에 명시된 워크플로우가 이 문서보다 우선한다. (예: `claude-history-viewer`는 GitHub Flow 사용)

## 브랜치 전략

기본값으로 Git Flow를 따른다.

- `main`: 배포 가능한 안정 브랜치
- `develop`: 개발 통합 브랜치
- `feature/<기능명>`: 기능 개발 브랜치
- `hotfix/<이슈명>`: 긴급 수정 브랜치

## feature 브랜치

1. develop 브랜치에서 최신화 (`git pull origin develop`)
2. `feature/<기능명>` 브랜치 생성
3. 작업 및 검증 (lint, build, dev 확인)
4. 커밋
5. feature → develop PR 생성 및 Merge

## hotfix 브랜치

1. main 브랜치에서 `hotfix/<이슈명>` 브랜치 생성
2. 긴급 수정 및 검증
3. 커밋
4. hotfix → main PR 생성 및 Merge
5. hotfix → develop PR 생성 및 Merge (동기화)

## Merge 정책

- 사용자가 직접 확인하겠다고 하지 않는 한 PR 생성 후 바로 Merge
