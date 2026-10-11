# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 저장소 성격

Claude Code 사용법을 익히기 위한 학습용 저장소입니다. 애플리케이션 코드, 빌드 시스템, 테스트, 린터가 없으므로 build/lint/test 명령도 없습니다.

## 구성

- `README.md` — 저장소 소개(한국어).
- `docs/` — Claude Code 기능을 정리한 한국어 Markdown 문서. 예: `docs/claude-code-language.md`(응답 언어를 `language` 설정으로 바꾸는 방법).

## 문서 작성 관례

`docs/`의 기존 문서를 따릅니다.

- 한국어로 작성하고, 설정 키·파일 경로·명령어 같은 기술 용어는 원문 그대로 백틱으로 감쌉니다.
- 파일 하나에 주제 하나. 파일명은 `claude-code-<주제>.md` 형식의 kebab-case입니다.
- 구성: `#` 제목 → 한두 문장 요약 → `##` 섹션(설정 방법, 위치, 적용 확인 등). 주의할 점은 `>` 인용 블록, 경로 비교는 표, 순서가 있는 절차는 번호 목록으로 씁니다.
