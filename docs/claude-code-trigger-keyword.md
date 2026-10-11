# Claude Code 트리거 키워드와 커스텀 슬래시 커맨드 만들기

자주 하는 작업을 Markdown 파일 하나로 정의해 두면 `/ship` 같은 슬래시 커맨드로 실행할 수 있습니다. `CLAUDE.md`에 트리거 키워드를 적어 두면 커맨드 대신 `커밋푸시` 같은 말로도 같은 작업을 시킬 수 있습니다.

> 슬래시 커맨드는 Claude Code가 직접 인식합니다. 트리거 키워드는 `CLAUDE.md` 지침을 Claude가 읽고 따르는 방식입니다. 정해진 절차대로 실행되어야 하는 작업이라면 슬래시 커맨드를 기본으로 쓰세요.

## 구성 요소

| 구성 요소 | 역할 | 예시 |
| --- | --- | --- |
| 커맨드 파일 | 작업 절차를 정의합니다. 파일명이 커맨드 이름이 됩니다. | `ship.md` → `/ship` |
| `CLAUDE.md` | 트리거 키워드를 말하면 커맨드 파일의 절차를 따르라고 지시합니다. | `커밋푸시`, `ship` |

## 커맨드 파일 작성

맨 위 frontmatter에 설명과 설정을 적고, 그 아래 본문에 Claude가 따를 절차를 씁니다. 아래는 git add, commit, push를 한 번에 처리하는 `ship.md`입니다.

```markdown
---
description: 변경 사항을 git add, commit, push까지 한 번에 처리
argument-hint: "[커밋 메시지(선택)]"
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*), Bash(git branch:*)
---

현재 변경 사항을 스테이징하고 커밋한 뒤 원격에 푸시합니다.

## 현재 상태

- 브랜치: !`git branch --show-current`
- 상태: !`git status --short`

## 절차

1. 변경 사항이 없으면 알리고 끝냅니다.
2. 민감한 파일이 있으면 먼저 물어보고, 그 외에는 `git add -A`로 스테이징합니다.
3. 인자가 있으면 커밋 메시지로 씁니다: `$ARGUMENTS`
4. `git commit` 후 `git push`합니다. 거절되면 강제 푸시하지 않고 멈춥니다.
```

| 항목 | 의미 |
| --- | --- |
| `description` | `/` 입력 시 커맨드 목록에 표시되는 설명 |
| `argument-hint` | 커맨드 뒤에 넣을 인자 안내 |
| `allowed-tools` | 이 커맨드를 실행하는 동안 권한 확인 없이 쓸 수 있는 도구 |
| `` !`명령어` `` | 커맨드를 실행할 때 명령어 결과를 본문에 미리 채워 넣음 |
| `$ARGUMENTS` | 커맨드 뒤에 입력한 내용 (예: `/ship 문서 추가` → `문서 추가`) |

## 트리거 키워드 등록

`CLAUDE.md`에 키워드와 따를 절차를 적습니다.

```markdown
## 트리거 키워드

사용자가 다음 중 하나를 말하면 `~/.claude/commands/ship.md`의 절차대로 git add → commit → push를 한 번에 실행합니다. 키워드 뒤에 붙은 문장이 있으면 커밋 메시지로 씁니다.

- `/ship` (슬래시 커맨드)
- `커밋푸시`, `ship`
```

## 파일 위치

| 적용 범위 | 커맨드 파일 | 트리거 키워드 |
| --- | --- | --- |
| 이 프로젝트만 | `<프로젝트>/.claude/commands/ship.md` | `<프로젝트>/CLAUDE.md` |
| 내 모든 프로젝트 | `~/.claude/commands/ship.md` | `~/.claude/CLAUDE.md` |

- 프로젝트 레벨 파일은 git으로 공유되므로 팀원도 같은 커맨드를 쓸 수 있습니다.
- 전역 파일은 홈 디렉토리에 있어 git에 올라가지 않습니다. 다른 PC에서 쓰려면 같은 경로로 복사합니다.
- 같은 이름의 커맨드를 두 위치에 함께 두면 어느 쪽이 실행되는지 헷갈립니다. 한 곳에서만 관리하세요.

## 적용 확인

1. 새 Claude Code 세션을 열거나 VSCode 확장을 다시 시작합니다. 이미 열려 있는 세션에는 적용되지 않습니다.
2. `/`를 입력했을 때 목록에 `ship`이 보이면 커맨드가 등록된 것입니다.
3. `커밋푸시`라고 말했을 때 같은 절차로 진행되면 트리거 키워드도 적용된 것입니다.
