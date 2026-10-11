---
description: 변경 사항을 git add, commit, push까지 한 번에 처리
argument-hint: "[커밋 메시지(선택)]"
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*), Bash(git branch:*)
---

현재 변경 사항을 스테이징하고 커밋한 뒤 원격에 푸시합니다.

## 현재 상태

- 브랜치: !`git branch --show-current`
- 상태: !`git status --short`
- 변경 내용: !`git diff HEAD --stat`
- 최근 커밋: !`git log --oneline -5`

## 절차

1. 위 상태에 변경 사항이 없으면 "커밋할 변경 사항이 없습니다"라고 알리고 끝냅니다.
2. `.env`, 키 파일, 자격 증명처럼 민감해 보이는 파일이 있으면 스테이징하지 말고 사용자에게 먼저 물어봅니다. 그 외에는 `git add -A`로 모두 스테이징합니다.
3. 커밋 메시지를 정합니다.
   - 인자가 있으면 그대로 씁니다: `$ARGUMENTS`
   - 인자가 없으면 `git diff --cached`를 보고 변경 내용을 한국어 한 줄로 요약해 씁니다.
4. `git commit`을 실행합니다.
5. `git push`로 현재 브랜치를 푸시합니다. upstream이 없으면 `git push -u origin <현재 브랜치>`를 씁니다. 푸시가 거절되면 강제 푸시하지 말고 원인을 알린 뒤 멈춥니다.
6. 커밋 해시, 메시지, 푸시한 브랜치를 짧게 보고합니다.
