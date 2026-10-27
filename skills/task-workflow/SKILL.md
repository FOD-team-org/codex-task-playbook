---
name: task-workflow
description: 사람 한 명과 Codex 하나로 작업을 진행하다 다음 단계가 막혔을 때 단계별 할 일과 대기 규칙을 확인한다.
---

# 작업 흐름
먼저 현재 단계를 확인한다(마지막 보고의 `상태:` 줄, 없으면 REQUIREMENTS.md·SPEC.md·DECISIONS.md의 진행 정도).
| 단계 | 할 일 |
|---|---|
| INTERVIEW | requirements-interview 스킬로 한 영역씩 묻고 답을 REQUIREMENTS.md에 원문 그대로 적는다 |
| RESEARCH·PRD_DRAFT·ARCH·PLANNING·SPEC | 해당 문서를 작업 폴더에 쓴다. SPEC은 spec-writing 스킬로 SPEC.md에 쓴다 |
| IMPLEMENT·FIX | SPEC.md대로 직접 구현·수정한다 |
| VERIFY | SPEC.md의 검증 명령을 셸로 실행한다 |
| REVIEW | review-checklist 스킬로 읽기 전용 검수하고 REVIEW.md에 쓴다 |
| DOCS | 실제 증거(실행한 명령·결과·커밋)를 DECISIONS.md에 기록한다 |
| CLEANUP | 띄운 서버·프로세스를 종료하고 임시 파일을 정리한다 |
| PICK | REQUIREMENTS.md에서 다음 미완 항목을 고른다 |
다음 단계로 넘어가기 전에 누락 증거를 보충한다. 유한 선택지·승인이 필요하면 judgment 스킬로 DECISIONS.md에 등록한다.
마지막 줄은 `상태: <현재 단계>`. 사용자 답을 기다리면 앞 줄에 `대기: 사용자`.

검증 명령은 SPEC.md에 적고 셸로 실행한다. 설정 판단은 검증 명령을 찾지 못했거나 사용자 설정 변경이 필요할 때만 요청한다.

원본에서 제외한 것: fodh 상태·전이 명령, 인박스 확인, 위임 대기(runId), 두뇌/수행자 분리와 위임, MERGE(워크트리 병합), recovering(원장 조회), 오토파일럿, 검증 프리셋 자동 감지.

출처: FOD-team-org/fod-harness skills/fodh-workflow/SKILL.md (원작자 동일, 외부 CLI 의존 제거)
