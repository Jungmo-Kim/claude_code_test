# Claude Code 출력 스타일 바꾸기 (`/output-style`)

`/output-style` 명령어로 Claude가 응답하는 톤과 형식을 바꿀 수 있습니다. 선택한 스타일은 세션 전체에 적용되며, 기본 제공 스타일 외에 직접 만든 스타일도 쓸 수 있습니다.

> Claude Code v2.1.269 이상이 필요합니다.

## 사용법

| 입력 | 동작 |
| --- | --- |
| `/output-style` | 사용 가능한 스타일 목록과 현재 스타일 표시 |
| `/output-style concise` | 해당 스타일로 변경 (대소문자 구분 없음) |
| `/config` → **Output style** | 메뉴에서 선택 |

## 기본 제공 스타일

| 스타일 | 특징 |
| --- | --- |
| Default | 스타일 지침을 추가하지 않는 기본 상태 |
| Proactive | 일상적인 결정은 묻지 않고 바로 진행. 권한 모드는 그대로이고, 데이터 삭제나 공유·운영 시스템 작업 전에는 여전히 확인 |
| Concise | 결과부터 말하고 서론·진행 설명·마무리 요약을 생략. 요청한 설명, 오류·보안·파괴적 작업 관련 출력은 자세히 유지 |
| Explanatory | Default처럼 작업하면서 짧은 `Insight` 블록을 덧붙임 (파일이 아닌 대화에만 표시) |
| Learning | Explanatory의 `Insight` 블록에 더해, 사용자가 직접 구현할 부분을 `TODO(human)`으로 남김 |

## 설정이 저장되는 위치

명령어나 메뉴로 고르면 프로젝트의 `.claude/settings.local.json`에 저장됩니다.

```json
{
  "outputStyle": "Explanatory"
}
```

| 적용 범위 | 파일 경로 |
| --- | --- |
| 이 프로젝트만 | `<프로젝트>/.claude/settings.local.json` |
| 내 모든 프로젝트 | `~/.claude/settings.json` |

두 곳에 모두 설정되어 있으면 프로젝트 설정이 우선합니다.

> 설정 파일의 값은 대소문자를 구분합니다. `explanatory`처럼 잘못 쓰면 오류 없이 Default로 적용됩니다.

## 커스텀 스타일 만들기

YAML frontmatter가 있는 Markdown 파일을 아래 위치에 둡니다. 파일명이 기본 스타일 이름이 됩니다.

| 적용 범위 | 디렉터리 |
| --- | --- |
| 내 모든 프로젝트 | `~/.claude/output-styles/` |
| 이 프로젝트만 | `<프로젝트>/.claude/output-styles/` |

```markdown
---
name: Korean Tutor
description: 개념을 단계별로 설명하는 스타일
keep-coding-instructions: true
---

모든 답변은 핵심 개념 → 예제 → 요약 순서로 작성하세요.
```

| frontmatter 키 | 의미 |
| --- | --- |
| `name` | 스타일 목록에 표시되는 이름. 생략하면 파일명 사용 |
| `description` | 스타일 목록에 표시되는 설명 |
| `keep-coding-instructions` | `true`면 Claude Code의 기본 코딩 지침 유지. 기본값 `false`라서 생략하면 코딩 지침이 빠짐 |

적용 순서는 다음과 같습니다.

1. 위 디렉터리에 스타일 파일을 만듭니다.
2. Claude Code를 다시 시작합니다. 세션 중에 만들거나 고친 파일은 재시작해야 반영됩니다.
3. `/output-style <이름>`을 실행하거나 `/config`에서 고릅니다.

## 다른 기능과의 차이

| 기능 | 역할 |
| --- | --- |
| 출력 스타일 | 세션 전체에서 *어떻게* 응답할지(톤, 형식, 역할) 결정 |
| `CLAUDE.md` | Claude가 *무엇을* 아는지(프로젝트 지식) 결정. 어떤 스타일을 쓰든 계속 적용 |
| Skill | 호출하거나 작업이 맞을 때만 불러옴 |
| Subagent | 자체 시스템 프롬프트를 쓰므로 출력 스타일이 적용되지 않음 |
| Hook | 실제로 실행되어 동작을 강제함 |

> 출력 스타일은 지침일 뿐이라 반드시 지켜진다는 보장이 없습니다. 꼭 지켜야 하는 동작은 Hook으로 설정하세요.

## 참고

- 공식 문서: [Output styles](https://code.claude.com/docs/en/output-styles)
