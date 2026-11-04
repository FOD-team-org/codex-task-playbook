# codex-task-playbook

실무 담당자의 요청을 받아 데이터로 작은 웹 서비스를 만드는 과제를 Codex와 함께 풀기 위한 작업 규칙과 템플릿 모음입니다. 과제 내용이나 정답은 들어 있지 않습니다. 진행 순서와 지시문 틀만 들어 있습니다.

## 쓰는 법

설치는 아래 "설치"를 따릅니다. 과제 폴더에 이미 AGENTS.md가 있으면 덮어쓰지 말고 기존 파일 아래에 이어 붙입니다. `~/.codex/config.toml` 등 환경이 제공한 설정은 수정하지 않습니다.

진행 순서와 사람이 볼 곳은 `docs/WORKFLOW.md`에 있습니다. REQUIREMENTS.md는 설치할 때 복사하지 않습니다. 문제 정의 세션에서 담당자 답과 과제 요구사항을 모아 Codex가 만듭니다.

시간이 없으면 템플릿을 채우지 않고 아래 다섯 줄로 지시해도 됩니다. 과제 파일 이름과 이유만 바꿉니다.

```
README.md 과제로 /tmp/playbook/templates/PROBLEM_PROMPT.md 첫 메시지대로 시작해라.
이 문제로 확정한다. 이유: (내 말로 한 줄)
(새 세션) /tmp/playbook/templates/BUILD_PROMPT.md 1단계대로 해라.
2단계대로 구현해라. 바꿀 점: 없음
(새 세션, 빌드가 끝난 뒤) /tmp/playbook/templates/EXPLAIN_PROMPT.md 대로 정리해라.
```

## 구성

| 파일 | 용도 |
|---|---|
| `AGENTS.md` | Codex 작업 규칙: 우선순위, 진행, 스킬 고르기, 환경과 비용, 테스트, 범위, 끝내기 전 순서, 마지막 보고 |
| `docs/WORKFLOW.md` | 기본 순서(단계마다 내가 볼 것·넘어가는 기준), 상황별 할 일, 사후 설명 체크 |
| `templates/PROBLEM_PROMPT.md` | 문제 정의 세션 지시 |
| `templates/BUILD_PROMPT.md` | 빌드 지시: 1단계 설계, 2단계 구현, 요구 추가·수정·판단 반영 |
| `templates/VERIFY_PROMPT.md` | 수정 없이 반례 중심으로 검증하는 지시, 제출 전 연습 |
| `templates/EXPLAIN_PROMPT.md` | 빌드 후 사람이 직접 설명할 수 있게 채팅으로 정리하는 지시 |
| `templates/REQUIREMENTS.md` | 담당자 답을 원문 그대로 적는 요구사항 정본 |
| `templates/ASSUMPTIONS.md` | Codex가 직접 정한 판단과 이유, 사람이 고친 표시 |
| `templates/DECISIONS.md` | 판단대기 → 사람의 답·이유 → 판단기록, 검증, 문제 기록 |
| `skills/problem-definition/` | 자료를 세어 사실·가설·갈림길을 내고, 질문 하나씩 의논해 PROBLEM.md로 진짜 문제를 확정 |
| `skills/design-doc/` | 코드보다 먼저 쓰는 DESIGN.md. 끝날 때 코드와 대조 |
| `skills/default-stack/` | 과제가 스택을 정하지 않았을 때의 React·NestJS·PostgreSQL 구조, 빌드 설정, 배포 파일 |
| `skills/judgment/` | 요구사항에 답이 없는 판단을 판단대기와 ASSUMPTIONS.md로 나누기 |
| `skills/ui-baseline/` | 화면 기준선과 사람이 올릴 때의 디자인 단계 |
| `skills/http-roundtrip-test/` | createApp + 빈 포트 + fetch로 하는 node:test 틀과 API 경로별 오류 사례 |
| `skills/data-edge-tests/` | 데이터 처리 함정, 이상 행·빈칸·기록 없는 대상 구분 |
| `skills/data-exposure-guard/` | 응답 필드 고르기, 개인정보 가리기, CSV 처리 |
| `skills/llm-feature-guard/` | LLM 호출 기능이 있을 때만: 프롬프트 구분, 출력 검사, 호출 한도, 가짜 모델 테스트 |
| `skills/security-check/` | 외부 입력이 닿는 곳을 실제 요청으로 점검 |
| `skills/mutation-check/` | 핵심 줄을 일부러 틀리게 바꿔 테스트가 잡는지 확인 |
| `skills/bug-fix/` | 재현 → 고장 지도에서 원인 좁히기 → 실패 테스트 먼저 → 최소 수정 → 확인 |
| `skills/counterexample-review/` | 반례 기반 검증 |
| `skills/how/` | "X는 어떻게 동작하나" 코드 구조 설명 |
| `skills/bro/` | 직전 답을 전문용어 없이 쉬운 말로 다시 말하기 |

## 설치

Codex 세션을 열기 전에 과제 폴더 루트에서 아래 스크립트를 한 번 실행합니다. 사람이 직접 치면 토큰이 들지 않습니다. Codex는 `$CODEX_HOME/skills`(CODEX_HOME이 없으면 `~/.codex/skills`)에 있는 스킬을 읽습니다. 그 위치에 쓸 수 없으면 작업 폴더의 `.agents/skills`에 복사합니다. 설치가 끝나면 Codex를 다시 켜야 스킬과 AGENTS.md가 읽힙니다.

```bash
set -e
PB=/tmp/playbook
if [ -d "$PB/.git" ]; then git -C "$PB" pull -q; else git clone -q --depth 1 https://github.com/FOD-team-org/codex-task-playbook.git "$PB"; fi
SK="${CODEX_HOME:-$HOME/.codex}/skills"
if ! mkdir -p "$SK" 2>/dev/null || [ ! -w "$SK" ]; then SK="$PWD/.agents/skills"; mkdir -p "$SK"; fi
for d in "$PB"/skills/*/; do
  n=$(basename "$d")
  if [ -e "$SK/$n" ]; then echo "skill skip: $n"; else cp -r "$d" "$SK/$n"; fi
done
if [ ! -f AGENTS.md ]; then
  cp "$PB/AGENTS.md" AGENTS.md; echo "AGENTS.md: created"
elif grep -q 'codex-task-playbook' AGENTS.md; then
  echo "AGENTS.md: already appended"
else
  printf '\n\n<!-- codex-task-playbook -->\n' >> AGENTS.md
  cat "$PB/AGENTS.md" >> AGENTS.md; echo "AGENTS.md: appended"
fi
for f in DECISIONS ASSUMPTIONS; do
  if [ -e "$f.md" ]; then echo "template skip: $f.md"; else cp "$PB/templates/$f.md" .; echo "template copied: $f.md"; fi
done
echo "skills dir: $SK"
echo "skills installed: $(for d in "$PB"/skills/*/; do [ -f "$SK/$(basename "$d")/SKILL.md" ] && echo x; done | wc -l)/$(ls -d "$PB"/skills/*/ | wc -l)"
```

### Codex에게 설치를 맡길 때

```
git clone --depth 1 https://github.com/FOD-team-org/codex-task-playbook.git /tmp/playbook 을 실행하고, /tmp/playbook/README.md의 "설치"대로 설치해.
```

Codex는 다음을 지킨다.

1. 명령이 샌드박스·승인·네트워크 때문에 막히면 설정을 바꾸거나 우회하지 않는다. 멈추고, 사람이 터미널에 직접 붙여 넣을 수 있게 스크립트를 그대로 보여 준다.
2. 덮어쓰지 않는다. `~/.codex/config.toml`, 샌드박스·승인 설정, 과제 파일은 건드리지 않는다. 패키지 설치, git commit·push도 하지 않는다.
3. 실행 결과를 아래 형식으로 보고하고 멈춘다. 빌드는 시작하지 않는다.

```
## 설치 결과
- 스킬: 설치 수/전체 수 (위치: ...), 건너뛴 폴더: 없음 또는 이름
- AGENTS.md: 새로 만듦 / 이어 붙임 / 이미 있음
- 템플릿: 복사한 파일, 건너뛴 파일
- 막힌 명령: 없음 또는 명령과 오류 한 줄
- 다음: Codex를 종료하고 다시 켜야 스킬과 AGENTS.md가 읽힙니다.
```

## 원칙

- 측정 조건은 gpt-6.1-sol, 추론 강도 medium입니다.
- 화면 확인과 손 계산 표본 대조는 사람이 합니다.

## 출처

각 SKILL.md 맨 아래에 출처가 있습니다.

- `judgment`: FOD-team-org/fod-harness
- `how`·`bro`·`bug-fix`: pstack(MIT, Copyright (c) 2026 Lauren Tan, https://github.com/cursor/plugins/tree/main/pstack). 이식 목록과 라이선스 전문은 `NOTICE.md`에 있습니다.
- `problem-definition`·`default-stack`: 직접 작성했습니다.
- `design-doc`: 직접 작성했습니다. 범위를 문서로 먼저 고정하는 방식은 fod-harness의 spec-writing을 따랐습니다.
- `security-check`·`data-exposure-guard`·`llm-feature-guard`·`http-roundtrip-test`·`data-edge-tests`·`mutation-check`·`ui-baseline`: 공개 보안·테스트·화면 자료를 참고해 새로 쓴 것입니다. 참고 목록은 `NOTICE.md`에 있습니다.
