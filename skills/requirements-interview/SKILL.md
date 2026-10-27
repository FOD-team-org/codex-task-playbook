---
name: requirements-interview
description: 실무 담당자에게 요구사항을 물어 REQUIREMENTS.md를 채울 때 18개 영역 질문 은행(references/bank.md)에서 질문을 고른다.
---

# 요구사항 인터뷰
실무 담당자에게 한 번에 한 영역씩 질문하고, 답은 원문 그대로 REQUIREMENTS.md에 적는다.

질문은 references/bank.md에서 고른다. 한 줄 형식은 `[필수|선택] ID | free 또는 choice: 보기 | 질문 | 추천: 추천값`이다.

## 지금 할 일
1. REQUIREMENTS.md를 열어 18 영역 중 어디까지 답을 받았는지 확인한다.
2. 답이 오면 문항마다 ID와 함께 원문을 기록한다. 사용자가 "추천대로"라고 답한 경우만 추천값을 적고 `(추천대로)`를 붙인다. 모호한 답은 기록하지 않는다.
3. 다음 영역의 질문을 번호·추천값과 함께 채팅으로 묻고 마지막 상태 줄 앞 `대기: 사용자`로 멈춘다. 답이 없으면 같은 질문을 그대로 다시 보여 준다.
4. choice 문항 중 바로 정할 수 없는 것은 judgment 스킬로 DECISIONS.md의 판단대기에 올리고, 채팅에 '판단대기 A<n>로 올렸다' 한 줄만 알린다.
5. 미룬 문항은 REQUIREMENTS.md의 `미확인 / 가정`에 ID와 이유를 적는다.

## 끝내는 조건
모든 필수 문항이 답을 받았거나 판단대기에 올라가 있다.

## 금지
답 추정·필수 문항 건너뛰기 금지. 은행 파일·템플릿을 바꾸지 않는다. 막히면 판단대기로 올린다.

원본에서 제외한 것: fodh interview 명령(status·record·next·defer·skip), 상태 전이, 두뇌 쓰기 제한, Q17-02(두뇌와 수행자 모델), Q17-01·Q17-03의 설정값 자리표시자(일반 추천값으로 바꿈).

출처: FOD-team-org/fod-harness templates/interview/bank.md, templates/prompts/INTERVIEW.md (원작자 동일, 외부 CLI 의존 제거)
