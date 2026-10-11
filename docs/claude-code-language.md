# Claude Code 응답 언어를 한국어로 설정하기

Claude Code 설정 파일에 `language` 항목을 넣으면 Claude가 그 언어로 답합니다.

> 바뀌는 것은 Claude의 답변 언어뿐입니다. 메뉴, 단축키 안내 같은 Claude Code 화면 문구는 영어 그대로입니다.

## 설정 방법

설정 파일에 `"language": "korean"`을 추가합니다. 파일에 이미 다른 항목이 있으면 그대로 두고 이 항목만 더합니다.

```json
{
  "theme": "dark",
  "language": "korean"
}
```

## 설정 파일 위치

| 적용 범위 | 파일 경로 |
| --- | --- |
| 내 모든 프로젝트 | `~/.claude/settings.json` |
| 이 프로젝트만 | `<프로젝트>/.claude/settings.json` |

## 적용 확인

1. 새 Claude Code 세션을 열거나 VSCode 확장을 다시 시작합니다. 이미 열려 있는 세션에는 적용되지 않습니다.
2. 영어로 질문했을 때 한국어로 답하면 제대로 적용된 것입니다.
