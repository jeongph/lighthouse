# tmux 완벽 가이드

## 목차

1. [tmux란?](#tmux란)
2. [설치 방법](#설치-방법)
3. [기본 개념](#기본-개념)
4. [핵심 명령어](#핵심-명령어)
5. [세션 관리](#세션-관리)
6. [윈도우 관리](#윈도우-관리)
7. [패널 관리](#패널-관리)
8. [복사 모드](#복사-모드)
9. [설정 커스터마이징](#설정-커스터마이징)
10. [실전 활용 팁](#실전-활용-팁)

---

## tmux란?

**tmux**(Terminal Multiplexer)는 하나의 터미널에서 여러 개의 터미널 세션을 생성하고 관리할 수 있게 해주는 도구입니다.

### 주요 장점

- **세션 유지**: SSH 연결이 끊어져도 작업이 유지됨
- **화면 분할**: 하나의 터미널을 여러 패널로 분할 가능
- **멀티태스킹**: 여러 작업을 동시에 진행하고 전환 가능
- **원격 협업**: 여러 사용자가 같은 세션에 접속 가능

---

## 설치 방법

### macOS
```bash
brew install tmux
```

### Ubuntu/Debian
```bash
sudo apt update && sudo apt install tmux
```

### CentOS/RHEL
```bash
sudo yum install tmux
```

### 버전 확인
```bash
tmux -V
```

---

## 기본 개념

tmux는 세 가지 계층 구조로 이루어져 있습니다:

```
Server (서버)
  └── Session (세션)
        └── Window (윈도우)
              └── Pane (패널)
```

| 구성 요소 | 설명 |
|-----------|------|
| **Server** | tmux 프로세스 전체를 관리 |
| **Session** | 독립적인 작업 공간 (프로젝트 단위) |
| **Window** | 세션 내의 탭 (작업 유형별 분리) |
| **Pane** | 윈도우 내의 분할된 화면 영역 |

---

## 핵심 명령어

### Prefix 키

tmux의 모든 단축키는 **Prefix 키**를 먼저 누른 후 사용합니다.

> **기본 Prefix**: `Ctrl + b`

예를 들어, 새 윈도우를 만들려면:
1. `Ctrl + b` 를 누르고 손을 뗀다
2. `c` 를 누른다

### 자주 사용하는 단축키 요약

| 단축키 | 동작 |
|--------|------|
| `Prefix + ?` | 단축키 목록 보기 |
| `Prefix + :` | 명령어 모드 진입 |
| `Prefix + d` | 세션에서 분리 (detach) |
| `Prefix + t` | 시계 표시 |

---

## 세션 관리

### 세션 생성

```bash
# 이름 없이 생성
tmux

# 이름을 지정하여 생성
tmux new -s 세션이름

# 새 세션을 생성하면서 특정 디렉토리에서 시작
tmux new -s 세션이름 -c ~/projects/myapp
```

### 세션 목록 확인

```bash
tmux ls
# 또는
tmux list-sessions
```

### 세션 연결 (Attach)

```bash
# 마지막 세션에 연결
tmux attach
# 또는
tmux a

# 특정 세션에 연결
tmux attach -t 세션이름
tmux a -t 세션이름
```

### 세션 분리 (Detach)

```bash
# 단축키
Prefix + d

# 명령어
tmux detach
```

### 세션 종료

```bash
# 현재 세션 종료
exit

# 특정 세션 종료
tmux kill-session -t 세션이름

# 모든 세션 종료
tmux kill-server
```

### 세션 이름 변경

```bash
# 단축키
Prefix + $

# 명령어
tmux rename-session -t 기존이름 새이름
```

### 세션 전환

```bash
# 단축키
Prefix + s    # 세션 목록에서 선택
Prefix + (    # 이전 세션
Prefix + )    # 다음 세션
```

---

## 윈도우 관리

### 윈도우 생성

| 단축키 | 설명 |
|--------|------|
| `Prefix + c` | 새 윈도우 생성 |

```bash
# 명령어로 생성
tmux new-window -n 윈도우이름
```

### 윈도우 전환

| 단축키 | 설명 |
|--------|------|
| `Prefix + n` | 다음 윈도우 |
| `Prefix + p` | 이전 윈도우 |
| `Prefix + 숫자` | 해당 번호 윈도우로 이동 (0-9) |
| `Prefix + w` | 윈도우 목록에서 선택 |
| `Prefix + l` | 마지막으로 사용한 윈도우로 이동 |

### 윈도우 이름 변경

| 단축키 | 설명 |
|--------|------|
| `Prefix + ,` | 현재 윈도우 이름 변경 |

### 윈도우 종료

| 단축키 | 설명 |
|--------|------|
| `Prefix + &` | 현재 윈도우 종료 (확인 필요) |

```bash
# 명령어
exit  # 마지막 패널에서 실행하면 윈도우 종료
```

### 윈도우 이동

```bash
# 윈도우 순서 변경
Prefix + :
swap-window -t 목표위치

# 예: 현재 윈도우를 0번 위치로 이동
swap-window -t 0
```

---

## 패널 관리

### 패널 분할

| 단축키 | 설명 |
|--------|------|
| `Prefix + %` | 수직 분할 (좌우로 나눔) |
| `Prefix + "` | 수평 분할 (상하로 나눔) |

```
수직 분할 (%)          수평 분할 (")
┌─────┬─────┐         ┌───────────┐
│     │     │         │           │
│     │     │         ├───────────┤
│     │     │         │           │
└─────┴─────┘         └───────────┘
```

### 패널 이동

| 단축키 | 설명 |
|--------|------|
| `Prefix + 방향키` | 해당 방향의 패널로 이동 |
| `Prefix + o` | 다음 패널로 순환 이동 |
| `Prefix + ;` | 마지막으로 활성화된 패널로 이동 |
| `Prefix + q` | 패널 번호 표시 후 번호로 이동 |

### 패널 크기 조절

| 단축키 | 설명 |
|--------|------|
| `Prefix + Ctrl + 방향키` | 해당 방향으로 1칸씩 크기 조절 |
| `Prefix + Alt + 방향키` | 해당 방향으로 5칸씩 크기 조절 |

```bash
# 명령어 모드에서
Prefix + :
resize-pane -D 10  # 아래로 10칸 확장
resize-pane -U 10  # 위로 10칸 확장
resize-pane -L 10  # 왼쪽으로 10칸 확장
resize-pane -R 10  # 오른쪽으로 10칸 확장
```

### 패널 레이아웃

| 단축키 | 설명 |
|--------|------|
| `Prefix + Space` | 기본 레이아웃 순환 |
| `Prefix + Alt + 1` | even-horizontal |
| `Prefix + Alt + 2` | even-vertical |
| `Prefix + Alt + 3` | main-horizontal |
| `Prefix + Alt + 4` | main-vertical |
| `Prefix + Alt + 5` | tiled |

### 패널 확대/축소

| 단축키 | 설명 |
|--------|------|
| `Prefix + z` | 현재 패널 전체화면 토글 |

### 패널 종료

| 단축키 | 설명 |
|--------|------|
| `Prefix + x` | 현재 패널 종료 (확인 필요) |

```bash
exit  # 또는 Ctrl + d
```

### 패널을 새 윈도우로 분리

| 단축키 | 설명 |
|--------|------|
| `Prefix + !` | 현재 패널을 새 윈도우로 분리 |

---

## 복사 모드

tmux에서 텍스트를 복사하려면 **복사 모드**에 진입해야 합니다.

### 복사 모드 진입/종료

| 단축키 | 설명 |
|--------|------|
| `Prefix + [` | 복사 모드 진입 |
| `q` 또는 `Esc` | 복사 모드 종료 |

### 복사 모드에서 이동

| 키 | 설명 |
|----|------|
| `방향키` | 커서 이동 |
| `h, j, k, l` | vim 스타일 이동 |
| `w, b` | 단어 단위 이동 |
| `Ctrl + u` | 반 페이지 위로 |
| `Ctrl + d` | 반 페이지 아래로 |
| `g` | 맨 위로 |
| `G` | 맨 아래로 |
| `/` | 앞으로 검색 |
| `?` | 뒤로 검색 |
| `n` | 다음 검색 결과 |
| `N` | 이전 검색 결과 |

### 텍스트 선택 및 복사

| 키 | 설명 |
|----|------|
| `Space` | 선택 시작 |
| `Enter` | 선택 영역 복사 후 복사 모드 종료 |
| `Prefix + ]` | 붙여넣기 |

### vi 모드 사용 (권장)

```bash
# ~/.tmux.conf에 추가
setw -g mode-keys vi

# vi 스타일 복사
# v로 선택 시작, y로 복사
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi y send-keys -X copy-selection-and-cancel
```

---

## 설정 커스터마이징

tmux 설정 파일: `~/.tmux.conf`

### 기본 설정 예시

```bash
# ~/.tmux.conf

# Prefix 키 변경 (Ctrl+a로 변경)
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# 마우스 지원 활성화
set -g mouse on

# 기본 쉘 설정
set -g default-shell /bin/zsh

# 히스토리 크기 증가
set -g history-limit 50000

# 윈도우/패널 인덱스를 1부터 시작
set -g base-index 1
setw -g pane-base-index 1

# 윈도우 번호 자동 재정렬
set -g renumber-windows on

# 256 색상 지원
set -g default-terminal "screen-256color"

# 상태바 설정
set -g status-position bottom
set -g status-style 'bg=#333333 fg=#ffffff'

# 패널 분할 단축키 직관적으로 변경
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
unbind '"'
unbind %

# 패널 이동 vim 스타일
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# 패널 크기 조절
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# 설정 파일 리로드
bind r source-file ~/.tmux.conf \; display "설정을 다시 불러왔습니다!"

# ESC 지연 제거 (vim 사용자용)
set -sg escape-time 0

# 포커스 이벤트 활성화
set -g focus-events on

# vi 모드
setw -g mode-keys vi
```

### 설정 적용

```bash
# tmux 외부에서
tmux source-file ~/.tmux.conf

# tmux 내부에서
Prefix + :
source-file ~/.tmux.conf

# 또는 위 설정대로 했다면
Prefix + r
```

---

## 실전 활용 팁

### 1. 프로젝트별 세션 관리

```bash
# 프로젝트 시작 시
tmux new -s project-name -c ~/projects/project-name

# 여러 프로젝트 동시 작업
tmux new -s frontend -c ~/projects/frontend
tmux new -s backend -c ~/projects/backend
```

### 2. 개발 환경 자동 설정 스크립트

```bash
#!/bin/bash
# ~/scripts/dev-setup.sh

SESSION="dev"

# 기존 세션이 있으면 연결
tmux has-session -t $SESSION 2>/dev/null
if [ $? == 0 ]; then
    tmux attach -t $SESSION
    exit 0
fi

# 새 세션 생성
tmux new-session -d -s $SESSION -c ~/projects/myapp

# 첫 번째 윈도우: 에디터
tmux rename-window -t $SESSION:1 'editor'
tmux send-keys -t $SESSION:1 'nvim .' C-m

# 두 번째 윈도우: 서버
tmux new-window -t $SESSION -n 'server'
tmux send-keys -t $SESSION:2 'npm run dev' C-m

# 세 번째 윈도우: Git
tmux new-window -t $SESSION -n 'git'
tmux send-keys -t $SESSION:3 'git status' C-m

# 첫 번째 윈도우로 이동 후 세션 연결
tmux select-window -t $SESSION:1
tmux attach -t $SESSION
```

### 3. SSH 연결 유지

```bash
# 원격 서버에서 tmux 실행
ssh user@server
tmux new -s work

# 작업 중 연결이 끊어져도 다시 연결 가능
ssh user@server
tmux attach -t work
```

### 4. 페어 프로그래밍

```bash
# 첫 번째 사용자가 세션 생성
tmux new -s pair

# 두 번째 사용자가 같은 세션에 연결
tmux attach -t pair

# 읽기 전용으로 연결
tmux attach -t pair -r
```

### 5. 로그 모니터링 분할 화면

```bash
# 여러 로그 파일 동시 모니터링
tmux new -s logs

# 패널 분할 후 각 패널에서
tail -f /var/log/app.log
tail -f /var/log/error.log
tail -f /var/log/access.log
```

### 6. 유용한 명령어 조합

```bash
# 모든 패널에 동시 입력
Prefix + :
setw synchronize-panes on

# 동시 입력 해제
setw synchronize-panes off

# 현재 패널 내용을 파일로 저장
Prefix + :
capture-pane -S -3000
save-buffer ~/tmux-output.txt

# 세션 공유 소켓 사용
tmux -S /tmp/shared new -s shared
chmod 777 /tmp/shared
# 다른 사용자가 연결
tmux -S /tmp/shared attach -t shared
```

### 7. 자주 쓰는 별칭 (Alias)

```bash
# ~/.bashrc 또는 ~/.zshrc에 추가
alias t='tmux'
alias ta='tmux attach -t'
alias tn='tmux new -s'
alias tl='tmux ls'
alias tk='tmux kill-session -t'
```

---

## 문제 해결

### 색상이 제대로 표시되지 않을 때

```bash
# ~/.tmux.conf
set -g default-terminal "tmux-256color"
set -ag terminal-overrides ",xterm-256color:RGB"
```

### 마우스 스크롤이 안 될 때

```bash
# ~/.tmux.conf
set -g mouse on
```

### 클립보드 연동 (macOS)

```bash
# ~/.tmux.conf
# macOS에서 시스템 클립보드 사용
bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "pbcopy"
bind-key -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "pbcopy"
```

---

## 참고 자료

- [tmux 공식 GitHub](https://github.com/tmux/tmux)
- [tmux man page](https://man7.org/linux/man-pages/man1/tmux.1.html)
- [Awesome tmux](https://github.com/rothgar/awesome-tmux) - 플러그인 및 리소스 모음
