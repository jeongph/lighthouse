# GitHub CLI (gh) 가이드

## 목차

- [TL;DR - 자주 쓰는 명령어](#tldr---자주-쓰는-명령어)
- [1. 설치](#1-설치)
  - [macOS](#macos)
  - [Linux](#linux)
  - [Windows](#windows)
- [2. 인증](#2-인증)
- [3. 저장소 (repo)](#3-저장소-repo)
- [4. Pull Request (pr)](#4-pull-request-pr)
- [5. Issue](#5-issue)
- [6. Release](#6-release)
- [7. Actions (workflow / run)](#7-actions-workflow--run)
- [8. Gist](#8-gist)
- [9. Search](#9-search)
- [10. API 직접 호출](#10-api-직접-호출)
- [11. 설정 및 별칭](#11-설정-및-별칭)
- [12. Extension](#12-extension)
- [References](#references)

---

## TL;DR - 자주 쓰는 명령어

```bash
# 인증
gh auth login                          # GitHub 로그인
gh auth status                         # 인증 상태 확인

# 저장소
gh repo clone <owner>/<repo>           # 저장소 클론
gh repo create <name> --public         # 새 저장소 생성
gh repo view --web                     # 브라우저에서 저장소 열기

# Pull Request
gh pr create --title "제목" --body "내용"  # PR 생성
gh pr create --fill                    # 커밋 메시지로 PR 자동 생성
gh pr list                             # PR 목록
gh pr checkout <number>                # PR 브랜치 체크아웃
gh pr merge <number>                   # PR 머지
gh pr view <number> --web              # 브라우저에서 PR 열기

# Issue
gh issue create --title "제목"          # 이슈 생성
gh issue list                          # 이슈 목록
gh issue close <number>                # 이슈 닫기

# Actions
gh run list                            # 워크플로우 실행 목록
gh run watch                           # 실행 중인 워크플로우 감시

# 기타
gh api <endpoint>                      # GitHub API 직접 호출
gh search repos <query>                # 저장소 검색
```

---

## 1. 설치

### macOS

```bash
# Homebrew
brew install gh

# 업그레이드
brew upgrade gh
```

### Linux

**Ubuntu / Debian**

```bash
(type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) \
  && sudo mkdir -p -m 755 /etc/apt/keyrings \
  && out=$(mktemp) \
  && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
  && cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
  && sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
  | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
  && sudo apt update \
  && sudo apt install gh -y
```

```bash
# 업그레이드
sudo apt update && sudo apt install gh
```

**Fedora (DNF5)**

```bash
sudo dnf install dnf5-plugins
sudo dnf config-manager addrepo --from-repofile=https://cli.github.com/packages/rpm/gh-cli.repo
sudo dnf install gh --repo gh-cli
```

**Arch Linux**

```bash
sudo pacman -S github-cli
```

**Homebrew (Linux)**

```bash
brew install gh
```

### Windows

```powershell
# WinGet
winget install --id GitHub.cli

# Scoop
scoop install gh

# Chocolatey
choco install gh
```

---

## 2. 인증

### 로그인

```bash
# 대화형 로그인 (권장)
gh auth login

# 브라우저 인증 + 코드 클립보드 복사
gh auth login --web --clipboard

# 토큰으로 로그인
echo "<token>" | gh auth login --with-token

# GitHub Enterprise Server
gh auth login --hostname enterprise.example.com

# Git 프로토콜 지정 (ssh 또는 https)
gh auth login --git-protocol ssh
```

`--with-token` 사용 시 최소 필요 scope: `repo`, `read:org`, `gist`

### 인증 관리

```bash
gh auth status             # 인증 상태 확인
gh auth token              # 현재 토큰 출력
gh auth refresh            # 토큰 갱신
gh auth switch             # 계정 전환 (여러 계정 사용 시)
gh auth logout             # 로그아웃
gh auth setup-git          # gh를 git credential helper로 설정
```

---

## 3. 저장소 (repo)

### 생성

```bash
# 공개 저장소 생성
gh repo create my-project --public

# 비공개 저장소 + 클론
gh repo create my-project --private --clone

# 현재 디렉토리를 저장소로
gh repo create --source=.
```

### 클론 / 포크

```bash
gh repo clone owner/repo
gh repo fork owner/repo              # 포크 생성
gh repo fork owner/repo --clone      # 포크 + 클론
```

### 조회 / 관리

```bash
gh repo list                         # 내 저장소 목록
gh repo list <owner>                 # 특정 사용자/조직 저장소 목록
gh repo view                         # 현재 저장소 정보
gh repo view owner/repo --web        # 브라우저에서 열기
gh repo rename <new-name>            # 이름 변경
gh repo edit --default-branch main   # 기본 브랜치 변경
gh repo sync                         # 포크를 upstream과 동기화
gh repo archive                      # 저장소 아카이브
gh repo delete <repo> --yes          # 저장소 삭제
```

---

## 4. Pull Request (pr)

### 생성

```bash
# 대화형
gh pr create

# 제목/본문 지정
gh pr create --title "기능 추가" --body "상세 설명"

# 커밋 메시지로 자동 채우기
gh pr create --fill

# Draft PR
gh pr create --draft --title "WIP: 기능 개발 중"

# 리뷰어, 라벨, 베이스 브랜치 지정
gh pr create --reviewer user1,user2 --label bug --base develop

# 브라우저에서 PR 작성
gh pr create --web
```

본문에 `Fixes #123` 또는 `Closes #123`을 포함하면 머지 시 해당 이슈가 자동으로 닫힌다.

### 조회

```bash
gh pr list                           # PR 목록
gh pr list --state closed            # 닫힌 PR
gh pr list --author @me              # 내 PR
gh pr view <number>                  # PR 상세 정보
gh pr view <number> --web            # 브라우저에서 열기
gh pr status                         # 현재 브랜치 관련 PR 상태
gh pr diff <number>                  # PR diff 확인
gh pr checks <number>                # CI 체크 상태
```

### 작업

```bash
gh pr checkout <number>              # PR 브랜치 체크아웃
gh pr merge <number>                 # 머지
gh pr merge <number> --squash        # 스쿼시 머지
gh pr merge <number> --rebase        # 리베이스 머지
gh pr merge <number> --delete-branch # 머지 후 브랜치 삭제
gh pr close <number>                 # PR 닫기
gh pr reopen <number>                # PR 다시 열기
gh pr ready <number>                 # Draft -> Ready 전환
gh pr review <number> --approve      # PR 승인
gh pr review <number> --comment -b "코멘트"
```

---

## 5. Issue

### 생성

```bash
# 대화형
gh issue create

# 제목/본문 지정
gh issue create --title "버그 리포트" --body "설명"

# 라벨, 담당자 지정
gh issue create --title "버그" --label bug --assignee @me

# 브라우저에서 작성
gh issue create --web
```

### 조회

```bash
gh issue list                        # 이슈 목록
gh issue list --label bug            # 라벨 필터
gh issue list --assignee @me         # 내 이슈
gh issue view <number>               # 이슈 상세
gh issue view <number> --web         # 브라우저에서 열기
gh issue status                      # 나와 관련된 이슈 상태
```

### 관리

```bash
gh issue close <number>              # 이슈 닫기
gh issue reopen <number>             # 이슈 다시 열기
gh issue edit <number> --title "새 제목"
gh issue comment <number> --body "코멘트"
gh issue pin <number>                # 이슈 고정
gh issue transfer <number> <repo>    # 다른 저장소로 이전
```

---

## 6. Release

```bash
# 릴리스 생성 (태그 기반)
gh release create v1.0.0

# 제목, 노트, 파일 첨부
gh release create v1.0.0 --title "v1.0.0" --notes "릴리스 노트" ./dist/*.zip

# Draft 릴리스
gh release create v1.0.0 --draft

# 자동 릴리스 노트 생성
gh release create v1.0.0 --generate-notes

# 조회
gh release list
gh release view v1.0.0

# 에셋 다운로드
gh release download v1.0.0

# 삭제
gh release delete v1.0.0 --yes
```

---

## 7. Actions (workflow / run)

### 워크플로우

```bash
gh workflow list                     # 워크플로우 목록
gh workflow view <name|id>           # 워크플로우 상세
gh workflow run <name|file>          # 수동 실행
gh workflow enable <name|id>         # 활성화
gh workflow disable <name|id>        # 비활성화
```

### 실행(Run) 관리

```bash
gh run list                          # 실행 목록
gh run view <run-id>                 # 실행 상세
gh run watch                         # 실행 중인 워크플로우 실시간 감시
gh run rerun <run-id>                # 재실행
gh run cancel <run-id>               # 취소
gh run download <run-id>             # 아티팩트 다운로드
```

---

## 8. Gist

```bash
# 생성
gh gist create file.txt                      # 비밀 gist
gh gist create file.txt --public             # 공개 gist
gh gist create file1.txt file2.txt           # 여러 파일

# 조회
gh gist list
gh gist view <id|url>

# 편집 / 삭제
gh gist edit <id>
gh gist delete <id>
```

---

## 9. Search

```bash
gh search repos <query>              # 저장소 검색
gh search repos "react" --language=typescript --stars=">1000"

gh search issues <query>             # 이슈 검색
gh search prs <query>                # PR 검색
gh search commits <query>            # 커밋 검색
gh search code <query>               # 코드 검색
```

---

## 10. API 직접 호출

`gh api`를 사용하면 인증 처리를 자동으로 해주므로 curl 대신 편리하게 사용할 수 있다.

```bash
# GET
gh api repos/{owner}/{repo}
gh api repos/{owner}/{repo}/pulls

# POST (JSON 필드 지정)
gh api repos/{owner}/{repo}/issues -f title="제목" -f body="내용"

# pagination 자동 처리
gh api repos/{owner}/{repo}/issues --paginate

# jq 필터
gh api repos/{owner}/{repo}/pulls --jq '.[].title'

# GraphQL
gh api graphql -f query='{ viewer { login } }'
```

`{owner}`와 `{repo}`는 현재 디렉토리의 저장소 정보로 자동 치환된다.

---

## 11. 설정 및 별칭

### 설정

```bash
gh config set editor vim             # 기본 에디터 설정
gh config set git_protocol ssh       # Git 프로토콜 설정
gh config set prompt disabled        # 프롬프트 비활성화
gh config list                       # 현재 설정 확인
```

### 별칭 (alias)

```bash
# 별칭 등록
gh alias set pv 'pr view'
gh alias set co 'pr checkout'

# 복잡한 별칭
gh alias set bugs 'issue list --label=bug'

# 별칭 목록 / 삭제
gh alias list
gh alias delete pv
```

### 환경 변수

| 변수 | 설명 |
|------|------|
| `GITHUB_TOKEN` | 인증 토큰 (gh auth login 대신 사용 가능) |
| `GH_HOST` | 기본 GitHub 호스트명 |
| `GH_ENTERPRISE_TOKEN` | GitHub Enterprise 인증 토큰 |
| `GH_EDITOR` | 에디터 지정 |
| `NO_COLOR` | 컬러 출력 비활성화 |

---

## 12. Extension

gh의 기능을 확장하는 커뮤니티 플러그인이다.

```bash
# 확장 검색 / 설치
gh extension browse                  # 브라우저에서 탐색
gh extension search <query>          # 확장 검색
gh extension install <repo>          # 설치

# 관리
gh extension list                    # 설치된 확장 목록
gh extension upgrade --all           # 전체 업그레이드
gh extension remove <name>           # 제거
```

유용한 확장 예시:
- `gh-dash` - 터미널 대시보드
- `gh-copilot` - Copilot CLI
- `gh-poi` - 머지된 로컬 브랜치 정리

---

## References

- [GitHub CLI 공식 사이트](https://cli.github.com/)
- [GitHub CLI 매뉴얼](https://cli.github.com/manual/)
- [GitHub CLI 저장소 (cli/cli)](https://github.com/cli/cli)
- [GitHub CLI Linux 설치 가이드](https://github.com/cli/cli/blob/trunk/docs/install_linux.md)
