# CLAUDE.md

이 파일은 Claude Code (claude.ai/code)가 이 저장소에서 작업할 때 참고하는 지침이다.

## 개요
이 디렉토리는 개인 개발 루트(dev)로, 여러 프로젝트 그룹을 포함하는 워크스페이스이다.

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
- 타입: feat, fix, refactor, docs, style, test, chore

## 워크플로우
- 브랜치 전략: Git Flow (main, develop, feature/<기능명>, hotfix/<이슈명>)
- Merge 정책: 사용자가 직접 확인하겠다고 하지 않는 한 PR 생성 후 바로 Merge

## 작업 히스토리
- 각 프로젝트의 `docs/history/` 디렉토리에 작업 기록
- 파일명: `yyyy-MM-dd-<author>-<작업명>.md`
- author: 작성자 구분 (claude, jeonguk 등)
- **작업 완료 시 반드시 히스토리 파일을 작성하고 커밋한다**

