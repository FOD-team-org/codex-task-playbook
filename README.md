# codex-task-playbook

실무 담당자의 요청을 받아 데이터로 작은 웹 서비스를 만드는 과제를 Codex와 함께 풀기 위한 작업 규칙과 템플릿 모음입니다. 과제 내용이나 정답은 들어 있지 않습니다. 진행 순서와 지시문 틀만 들어 있습니다.

## 쓰는 법

```bash
git clone https://github.com/kms970/codex-task-playbook.git /tmp/playbook
cp /tmp/playbook/AGENTS.md ./AGENTS.md          # 작업 폴더 루트에 둔다
cp /tmp/playbook/templates/REQUIREMENTS.md ./
cp /tmp/playbook/templates/DECISIONS.md ./
```

과제 폴더에 이미 AGENTS.md가 있으면 덮어쓰지 마세요. 기존 파일 아래에 이 내용을 이어 붙입니다.

## 구성

| 파일 | 용도 |
|---|---|
| `AGENTS.md` | Codex 작업 규칙(질문 없이 진행, 최소 구현, 브라우저 자동화 금지) |
| `templates/INTERVIEW.md` | 담당자에게 한 주제씩 물을 질문 목록 |
| `templates/REQUIREMENTS.md` | 담당자 답을 원문 그대로 적는 요구사항 정본 |
| `templates/BUILD_PROMPT.md` | 빌드 세션 첫 지시 |
| `templates/VERIFY_PROMPT.md` | 새 세션에서 수정 없이 반례 중심으로 검증하는 지시 |
| `templates/DECISIONS.md` | 결정과 이유, 검증 기록(사후 설명 준비) |
| `docs/WORKFLOW.md` | 180분 진행 순서 |
| `skills/counterexample-review/SKILL.md` | 반례 기반 검증 Skill |
| `skills/task-workflow/SKILL.md` | 사람+Codex 1개 진행 순서(단계별 할 일·대기 규칙) |
| `skills/requirements-interview/` | 담당자 인터뷰 Skill과 18개 영역 질문 은행(`references/bank.md`) |
| `skills/spec-writing/SKILL.md` | SPEC.md 범위·수용 기준 작성 |
| `skills/review-checklist/SKILL.md` | 검수 결과(SHA·결함 근거·판정)를 REVIEW.md에 기록 |
| `skills/judgment/SKILL.md` | 막힌 선택·승인을 DECISIONS.md 판단대기에 등록 |

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
