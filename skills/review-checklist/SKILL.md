---
name: review-checklist
description: 산출물을 검수할 때 대상 SHA와 결함 근거 및 판정을 REVIEW.md에 기록한다.
---

# 검수
검수 요청과 결과를 작업 폴더의 REVIEW.md에 쓴다.
산출물은 수정하지 않고 읽기 전용으로 검수한다.
머리에 대상 커밋 SHA·모델·effort를 기록한다.
높음: 핵심 동작 실패·데이터 손실. 중간: 제한된 조건의 기능 결함. 낮음: 사용성·유지보수 문제.
각 결함에 파일:줄, 재현 입력·순서, 실제 결과와 기대 결과, 수정안을 쓴다.
판정은 APPROVE 또는 REQUEST_CHANGES. 미실행 검증을 명시한다.
대상 SHA가 바뀌면 통과를 재사용하지 않는다. 높음 0 증거가 있어야 다음 단계로 간다.

원본에서 제외한 것: 문서 뼈대 명령, 수행자 교차검수(Claude↔Codex), 서브에이전트 모델 기록.

출처: FOD-team-org/fod-harness skills/fodh-review-checklist/SKILL.md (원작자 동일, 외부 CLI 의존 제거)
