# codex-task-playbook

실무 담당자의 요청을 받아 데이터로 작은 웹 서비스를 만드는 과제를 Codex와 함께 풀기 위한 작업 규칙과 템플릿 모음입니다. 과제 내용이나 정답은 들어 있지 않습니다. 진행 순서와 지시문 틀만 들어 있습니다.

## 쓰는 법

```bash
git clone https://github.com/FOD-team-org/codex-task-playbook.git /tmp/playbook
cp /tmp/playbook/AGENTS.md ./AGENTS.md          # 작업 폴더 루트에 둔다
cp /tmp/playbook/templates/REQUIREMENTS.md ./
cp /tmp/playbook/templates/DECISIONS.md ./
cp /tmp/playbook/templates/ASSUMPTIONS.md ./
```

과제 폴더에 이미 AGENTS.md가 있으면 덮어쓰지 마세요. 기존 파일 아래에 이 내용을 이어 붙입니다. `~/.codex/config.toml` 등 환경이 제공한 설정은 수정하지 않습니다.

REQUIREMENTS.md에 없는 보안·결제·운영 로직처럼 틀리면 서비스에 문제가 생기는 판단만 Codex가 DECISIONS.md `## 판단대기`에 추천안과 함께 올리고, 사람이 답과 이유를 적습니다. 답이 적힌 항목은 `## 판단기록`으로 옮기고 반영 위치를 남깁니다. 그 밖의 판단은 Codex가 정하고 ASSUMPTIONS.md에 이유와 함께 적습니다. 품질 검수와 사후 설명은 두 기록을 근거로 합니다.

## 구성

| 파일 | 용도 |
|---|---|
| `AGENTS.md` | Codex 작업 규칙(질문 없이 진행, 설계 먼저, 최소 구현, 브라우저 자동화 금지) |
| `templates/INTERVIEW.md` | 담당자에게 한 주제씩 물을 질문 목록 |
| `templates/REQUIREMENTS.md` | 담당자 답을 원문 그대로 적는 요구사항 정본 |
| `templates/BUILD_PROMPT.md` | 빌드 지시: 1단계 설계(DESIGN.md만 쓰고 멈춤), 2단계 구현, 수정·판단 반영 |
| `templates/VERIFY_PROMPT.md` | 새 세션에서 수정 없이 반례 중심으로 검증하는 지시 |
| `templates/ASSUMPTIONS.md` | Codex가 직접 정한 판단과 이유, 사람이 고친 표시 |
| `templates/DECISIONS.md` | 판단대기 → 사람의 답·이유 → 판단기록·반영 위치, 검증 채택/기각(사후 설명 준비) |
| `docs/WORKFLOW.md` | 180분 진행 순서 |
| `skills/counterexample-review/SKILL.md` | 반례 기반 검증 Skill |
| `skills/task-workflow/SKILL.md` | 사람+Codex 1개 진행 순서(단계별 할 일·대기 규칙) |
| `skills/requirements-interview/` | 담당자 인터뷰 Skill과 18개 영역 질문 은행(`references/bank.md`) |
| `skills/design-doc/SKILL.md` | 코드보다 먼저 쓰는 DESIGN.md: 흐름, 파일 지도, 요구사항→함수→테스트 표, 위험 지점, 고장 지도. 끝날 때 코드와 대조 |
| `skills/spec-writing/SKILL.md` | 수정 작업의 SPEC.md 범위·수용 기준 작성 |
| `skills/review-checklist/SKILL.md` | 검수 결과(SHA·결함 근거·판정)를 REVIEW.md에 기록 |
| `skills/judgment/SKILL.md` | 중요 판단(보안·결제·운영)만 판단대기에 등록, 나머지는 ASSUMPTIONS.md |
| `skills/in-your-own-words/SKILL.md` | 사용자의 목표와 풀려는 문제를 내 말로 다시 적기 |
| `skills/sharpen/SKILL.md` | 과제를 더 크게가 아니라 더 깊게 다시 쓰기 |
| `skills/tdd/SKILL.md` | 싼 테스트 경로가 있는 버그를 실패 테스트 먼저 쓰고 고치기 |
| `skills/blast-radius/SKILL.md` | 변경이 다른 곳에서 깨뜨릴 것을 찾고, 안전 근거 하나를 실행으로 증명 |
| `skills/bro/SKILL.md` | 직전 답을 전문용어 없이 쉬운 말로 다시 말하기 |
| `skills/how/` | "X는 어떻게 동작하나" 코드 구조 설명(탐색·설명 틀 `references/`) |
| `skills/investigation/SKILL.md` | 코드 변경 없는 조사: 근거를 단 설명이나 추천 |
| `skills/bug-fix/SKILL.md` | 직접 재현 → 원인 이분 탐색 → 최소 수정 → 같은 경로로 검증 |
| `skills/prototype/SKILL.md` | 설계 결정 하나를 위한 버리는 시제품과 변형 비교 |
| `skills/engineering-principles/SKILL.md` | 최소 diff·먼저 빼기·실물 검증·근본 원인 등 작업 원칙 8개 묶음 |
| `skills/ui-baseline/SKILL.md` | 화면 기준선(0단계 담백 기본)과 사람이 올릴 때의 1단계 정돈·2단계 브랜드 범위 |
| `skills/security-check/SKILL.md` | 완성 후·검증 때 외부 입력이 닿는 곳(형식·정규식·경로·XSS·오류 노출·인증·개인정보)을 실제 요청으로 점검 |
| `skills/data-exposure-guard/SKILL.md` | 응답 필드 고르기, 개인정보 가리기, 조회 실패 문구 통일, CSV BOM·따옴표·수식 문자 처리 |
| `skills/llm-feature-guard/SKILL.md` | LLM 호출 기능이 있을 때만: 프롬프트 구분, 출력 검사, 호출 한도, 가짜 모델 테스트 |
| `skills/http-roundtrip-test/SKILL.md` | createApp + 빈 포트 + fetch로 하는 node:test 틀과 라우트별 오류 사례 |
| `skills/data-edge-tests/SKILL.md` | BOM·빈 값·숫자·날짜·정렬·묶기·중복·평균 등 데이터 처리 함정과 테스트 |
| `skills/mutation-check/SKILL.md` | 핵심 줄을 일부러 틀리게 바꿔 테스트가 잡는지 확인하고 되돌리기 |

## 스킬 설치

Codex는 `$CODEX_HOME/skills`(CODEX_HOME이 없으면 `~/.codex/skills`)에 있는 스킬을 읽습니다. `skills/`의 폴더를 그 위치로 복사합니다.

```bash
mkdir -p ~/.codex/skills
cp -r /tmp/playbook/skills/* ~/.codex/skills/
```

`task-workflow`·`requirements-interview`·`spec-writing`·`review-checklist`·`judgment`는 FOD-team-org/fod-harness의 스킬을 외부 CLI 없이 쓰도록 옮긴 것입니다. 각 SKILL.md 맨 아래에 원본 경로가 있습니다.

## 원칙

- 모델 하나로 진행합니다. 빌드 세션 1개와 검증 세션 1~2개만 씁니다.
- AI 검증의 지적은 반례 행을 사람이 직접 확인한 뒤 채택합니다.
- 화면 확인과 손 계산 표본 대조는 사람이 합니다.

## 출처

- `task-workflow`·`requirements-interview`·`spec-writing`·`review-checklist`·`judgment`: FOD-team-org/fod-harness
- `in-your-own-words`·`sharpen`·`tdd`·`blast-radius`·`bro`·`how`·`investigation`·`bug-fix`·`prototype`·`engineering-principles`: pstack(MIT, Copyright (c) 2026 Lauren Tan, https://github.com/cursor/plugins/tree/main/pstack). 이식 목록과 라이선스 전문은 `NOTICE.md`에 있습니다.
- `design-doc`: 직접 작성했습니다. 범위를 문서로 먼저 고정하는 방식은 fod-harness의 spec-writing을 따랐습니다.
- `security-check`·`data-exposure-guard`·`llm-feature-guard`·`http-roundtrip-test`·`data-edge-tests`·`mutation-check`: 공개 보안·테스트 자료를 참고해 새로 쓴 것입니다. 참고 목록은 `NOTICE.md`에 있습니다.
