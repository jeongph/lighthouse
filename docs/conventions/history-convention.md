# 히스토리 문서 컨벤션

## 파일명

```
yyyy-MM-dd-<NNN>-<author>-<작업명>.md
```

- `NNN`: 해당 일자 내 3자리 순번 (001부터 시작)
- `author`: 작성자 구분 (claude, jeonguk 등)
- `작업명`: 작업 내용을 요약한 한글 kebab-case

## Frontmatter

모든 히스토리 문서는 YAML frontmatter로 시작한다.

```yaml
---
title: <작업 제목>
description: <한 줄 요약>
date: yyyy-MM-dd
author: claude | jeonguk
project: <프로젝트명 | "lighthouse">
tags:
  - feat | fix | refactor | style | docs | test | chore
---
```

| 필드 | 필수 | 설명 |
|------|------|------|
| title | O | 작업 제목 |
| description | O | 작업 내용 한 줄 요약 |
| date | O | 작업 완료일 (yyyy-MM-dd) |
| author | O | 작성자 (claude, jeonguk 등) |
| project | O | 대상 프로젝트명. 워크스페이스 자체 작업이면 `lighthouse` |
| tags | O | 작업 성격 분류. 커밋 타입과 동일한 값 사용 |

## 본문

frontmatter 아래에 문서 제목(`# 제목`)을 먼저 쓰고, Why → How → What 순서로 작성한다.

```markdown
# <작업 제목>

## Why
- 어떤 문제가 있었는지, 어떤 요구사항이 있었는지

## How
- 어떤 접근 방식과 도구를 사용해서 해결했는지
- 사용자가 내린 주요 선택/결정 사항
- 중간에 발생한 에러나 트러블슈팅 과정

## What
- 최종적으로 어떻게 완료되었는지 (변경된 파일, 적용된 설정 등)
```

## 전체 템플릿

```markdown
---
title: 워크스페이스 마이그레이션
description: jeongph-workspace를 lighthouse로 통합
date: 2026-02-04
author: claude
project: lighthouse
tags:
  - chore
---

# 워크스페이스 마이그레이션

## Why

워크스페이스가 분산되어 관리가 비효율적이었다.

## How

1. 기존 워크스페이스 백업
2. 히스토리 merge로 커밋 이력 보존
3. CLAUDE.md, README.md 재작성

## What

- lighthouse를 단일 워크스페이스 루트로 전환
- 커밋 히스토리 통합 완료
```
