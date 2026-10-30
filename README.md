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
| `AGENTS.md` | Codex 작업 규칙(우선순위, 과제가 정한 스택 우선, 설계 먼저, 최소 구현, 브라우저 자동화 금지, 마지막 보고 형식) |
| `templates/INTERVIEW.md` | 담당자에게 한 주제씩 물을 질문 목록 |
| `templates/REQUIREMENTS.md` | 담당자 답을 원문 그대로 적는 요구사항 정본, 만들지 않는 것, 아직 못 물은 것 |
| `templates/BUILD_PROMPT.md` | 빌드 지시: 1단계 설계(DESIGN.md만 쓰고 멈춤), 2단계 구현, 요구 추가·수정·판단 반영 |
| `templates/VERIFY_PROMPT.md` | 새 세션에서 수정 없이 반례 중심으로 검증하는 지시 |
| `templates/ASSUMPTIONS.md` | Codex가 직접 정한 판단과 이유, 사람이 고친 표시 |
| `templates/DECISIONS.md` | 판단대기 → 사람의 답·이유 → 판단기록·반영 위치, 검증 채택/기각(사후 설명 준비) |
| `docs/WORKFLOW.md` | 기본 순서(단계마다 내가 볼 것·넘어가는 기준)와 상황별 할 일 |
| `skills/design-doc/SKILL.md` | 코드보다 먼저 쓰는 DESIGN.md: 흐름, 파일 지도, 요구사항→함수→테스트 표, 위험 지점, 고장 지도. 끝날 때 코드와 대조 |
| `skills/judgment/SKILL.md` | 중요 판단(보안·결제·운영)만 판단대기에 등록, 나머지는 ASSUMPTIONS.md |
| `skills/bug-fix/SKILL.md` | 재현 → 고장 지도에서 원인 좁히기 → 실패 테스트 먼저 → 최소 수정 → 같은 방법으로 확인 |
| `skills/counterexample-review/SKILL.md` | 반례 기반 검증 |
| `skills/security-check/SKILL.md` | 완성 후·검증 때 외부 입력이 닿는 곳(형식·정규식·경로·XSS·오류 노출·인증·개인정보)을 실제 요청으로 점검 |
| `skills/data-exposure-guard/SKILL.md` | 응답 필드 고르기, 개인정보 가리기, 조회 실패 문구 통일, CSV BOM·따옴표·수식 문자 처리 |
| `skills/llm-feature-guard/SKILL.md` | LLM 호출 기능이 있을 때만: 프롬프트 구분, 출력 검사, 호출 한도, 가짜 모델 테스트 |
| `skills/http-roundtrip-test/SKILL.md` | createApp + 빈 포트 + fetch로 하는 node:test 틀과 라우트별 오류 사례 |
| `skills/data-edge-tests/SKILL.md` | BOM·빈 값·숫자·날짜·정렬·묶기·중복·평균 등 데이터 처리 함정과 테스트 |
| `skills/mutation-check/SKILL.md` | 핵심 줄을 일부러 틀리게 바꿔 테스트가 잡는지 확인하고 되돌리기 |
| `skills/ui-baseline/SKILL.md` | 화면 기준선(0단계 담백 기본)과 사람이 올릴 때의 1단계 정돈·2단계 브랜드 범위 |
| `skills/how/` | "X는 어떻게 동작하나" 코드 구조 설명(탐색·설명 틀 `references/`) |
| `skills/bro/SKILL.md` | 직전 답을 전문용어 없이 쉬운 말로 다시 말하기 |

## 스킬 설치

Codex는 `$CODEX_HOME/skills`(CODEX_HOME이 없으면 `~/.codex/skills`)에 있는 스킬을 읽습니다. `skills/`의 폴더를 그 위치로 복사합니다.

```bash
mkdir -p ~/.codex/skills
cp -r /tmp/playbook/skills/* ~/.codex/skills/
```

Codex 세션을 열기 전에 설치하고, `ls ~/.codex/skills`(또는 `ls "$CODEX_HOME/skills"`)로 폴더 13개가 있는지 확인합니다. 스킬은 AGENTS.md와 빌드 지시에 이름이 적혀 있어 따로 부르지 않아도 읽힙니다. `bug-fix`·`counterexample-review`는 수정·검증 지시 양식에, `how`·`bro`는 사람이 필요할 때 이름을 넣어 부릅니다.

## AI용 설치 절차

Codex에게 설치를 맡길 때는 이렇게 지시합니다. 사람이 위 명령을 직접 치면 토큰이 들지 않습니다.

```
git clone --depth 1 https://github.com/FOD-team-org/codex-task-playbook.git /tmp/playbook 을 실행하고, /tmp/playbook/README.md의 "AI용 설치 절차"대로 설치해.
```

Codex는 아래 순서를 따른다.

1. 과제 폴더 루트(지금 작업 폴더)에서 아래 스크립트를 한 번 실행한다. bash 기준이다.
2. 명령이 샌드박스·승인·네트워크 때문에 막히면 설정을 바꾸거나 우회하지 않는다. 멈추고, 사람이 터미널에 직접 붙여 넣을 수 있게 이 스크립트를 그대로 보여 준다.
3. 덮어쓰지 않는다. 같은 이름의 스킬 폴더와 기존 템플릿 파일은 건너뛰고, 기존 AGENTS.md에는 뒤에 이어 붙인다.
4. `~/.codex/config.toml`, 샌드박스·승인 설정, 과제 파일은 건드리지 않는다. 패키지 설치, git commit·push도 하지 않는다.
5. 실행 결과를 아래 형식으로 보고하고 멈춘다. 빌드는 시작하지 않는다.

```bash
set -e
PB=/tmp/playbook
if [ -d "$PB/.git" ]; then git -C "$PB" pull -q; else git clone -q --depth 1 https://github.com/FOD-team-org/codex-task-playbook.git "$PB"; fi
SK="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$SK"
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
for f in REQUIREMENTS DECISIONS ASSUMPTIONS; do
  if [ -e "$f.md" ]; then echo "template skip: $f.md"; else cp "$PB/templates/$f.md" .; echo "template copied: $f.md"; fi
done
echo "skills dir: $SK"
echo "skills installed: $(for d in "$PB"/skills/*/; do [ -f "$SK/$(basename "$d")/SKILL.md" ] && echo x; done | wc -l)/13"
```

보고 형식:

```
## 설치 결과
- 스킬: N/13 (위치: ...), 건너뛴 폴더: 없음 또는 이름
- AGENTS.md: 새로 만듦 / 이어 붙임 / 이미 있음
- 템플릿: 복사한 파일, 건너뛴 파일
- 막힌 명령: 없음 또는 명령과 오류 한 줄
- 다음: Codex를 종료하고 다시 켜야 스킬과 AGENTS.md가 읽힙니다.
```

빌드 지시는 `/tmp/playbook/templates/BUILD_PROMPT.md`, 질문 목록은 `INTERVIEW.md`, 검증 지시는 `VERIFY_PROMPT.md`에서 복사해 씁니다.

각 SKILL.md 맨 아래에 출처가 있습니다.

## 원칙

- 모델 하나로 진행합니다. 빌드 세션 1개와 검증 세션 1개만 씁니다.
- AI 검증의 지적은 반례 행을 사람이 직접 확인한 뒤 채택합니다.
- 화면 확인과 손 계산 표본 대조는 사람이 합니다.

## 출처

- `judgment`: FOD-team-org/fod-harness
- `how`·`bro`·`bug-fix`: pstack(MIT, Copyright (c) 2026 Lauren Tan, https://github.com/cursor/plugins/tree/main/pstack). 이식 목록과 라이선스 전문은 `NOTICE.md`에 있습니다.
- `design-doc`: 직접 작성했습니다. 범위를 문서로 먼저 고정하는 방식은 fod-harness의 spec-writing을 따랐습니다.
- `security-check`·`data-exposure-guard`·`llm-feature-guard`·`http-roundtrip-test`·`data-edge-tests`·`mutation-check`·`ui-baseline`: 공개 보안·테스트·화면 자료를 참고해 새로 쓴 것입니다. 참고 목록은 `NOTICE.md`에 있습니다.
