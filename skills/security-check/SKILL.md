---
name: security-check
description: Express 웹 서비스를 다 만든 뒤, 또는 검증할 때 사용한다. 외부 입력이 닿는 곳을 정해진 목록으로 찾아 실제 요청으로 확인하고, 확인된 보안 결함만 고치거나 보고한다. 과제 규모를 넘는 보안 장치는 더하지 않는다.
---

# 보안 점검

빌드 세션이면 결함을 고치고, 결함마다 테스트를 하나씩 더한다. 검증 세션이면 코드를 고치지 않고 보고만 한다. 요구사항에 적혀 있지 않은 항목도 점검한다.

## 1. 입력 목록
외부에서 오는 값만 의심한다. req.query, req.params, req.body, 헤더, 업로드 파일, data/ 파일의 셀 값이 여기에 든다. 환경 변수, 코드 상수, 서버가 계산한 값은 신뢰한다. 라우트마다 쓰는 입력을 한 줄씩 적는다.

## 2. 점검 항목
항목마다 해당 코드를 찾고, 적힌 요청을 실제로 보내 응답을 확인한다.
- 형식: query 값이 늘 문자열이라고 가정하는 곳. `?q=a&q=b`는 배열로 들어온다(Express 4는 `?q[x]=1`도 객체로 만든다). 문자열이 아니면 400을 준다.
- 정규식: 사용자 값이 `new RegExp(`에 들어가는 곳. `?q=(`를 보내면 SyntaxError로 500이 난다. 검색은 정규화한 문자열의 includes로 한다. 정규식이 꼭 필요하면 이스케이프하고, 실패하면 400을 준다.
- 경로 이탈: 사용자 값이 path.join, path.resolve, readFile, sendFile에 들어가는 곳. `../`, `..%2f`, 절대 경로를 보내 본다. 허용하는 파일 이름 목록으로 고르는 것이 가장 낫다. 그게 안 되면 path.resolve 결과가 허용 폴더 안인지 확인하고, 밖이면 404를 준다.
- 화면 XSS: public의 .js 파일과 .html 안 스크립트에서 innerHTML, outerHTML, insertAdjacentHTML, document.write에 데이터 값이 들어가는 곳. 데이터 셀에 `<img src=x onerror=alert(1)>`가 있다고 생각하고 따라간다. textContent와 createElement로 바꾼다. 서버가 템플릿 문자열로 HTML을 조립해 res.send하는 곳도 같은 문제다.
- 오류 노출: 인자 4개짜리 마지막 오류 처리기가 없는 곳, 또는 응답에 err.stack이나 err.message를 넣는 곳. Express 기본 처리기는 NODE_ENV가 production이 아니면 스택을 보낸다. 깨진 JSON 본문으로 POST해 본다. 응답은 고정 문구와 상태 코드만 보내고, 깨진 JSON에는 400을 준다.
- 인증: 관리자 경로와 전체 데이터 경로의 비교문. 헤더 없음, 빈 값, 비밀값 환경 변수가 없는 상태로 각각 요청해 본다. `req.headers[...] !== process.env.X`는 둘 다 없으면 통과한다. 비밀값이 설정되지 않았으면 경로를 닫는다(요구사항이 정한 응답, 없으면 404). 비교는 crypto.timingSafeEqual로 한다. 길이가 다르면 예외가 나므로 양쪽을 sha256 해시로 바꾼 뒤 비교한다.
- 입력 범위: 상태 값, 정렬 기준, 개수 같은 입력을 서버에서 허용 목록과 범위로 검사하는지 본다. 본문을 통째로 저장하거나 Object.assign으로 합치지 않고, 허용된 필드만 꺼낸다.
- 개인정보: 응답에 원본 행을 통째로 보내는 곳. 응답 본문에서 전체 전화번호와 주소를 찾아본다. 처리 방법은 data-exposure-guard 스킬을 따른다.
- 비밀값: 코드, 응답, 로그에 키·토큰·비밀번호 문자열이 있는지 본다.

## 3. 더하지 않는 것
요구사항에 없으면 helmet, bcrypt, HTTPS·HSTS, CSRF 토큰, 로그인 기능, npm audit, 외부 스캐너를 넣지 않는다. 호출 횟수 제한은 요구사항에 있거나 호출마다 비용이 드는 경로(LLM 호출 등)에만 메모리 카운터로 둔다. 인증을 걸지, 무엇을 가릴지처럼 요구사항에 없는 보안 정책은 judgment 스킬로 판단대기에 올린다.

## 4. 보고
항목마다 셋 중 하나로 적는다.
- 문제: 파일:줄, 보낸 요청, 실제 응답, 고친 내용(빌드 세션)
- 문제 없음: 확인한 요청과 응답
- 못 봄: 이유

위험해 보이는 코드만으로는 결함이 아니다. 입력이 그 줄까지 실제로 닿는지 따라가고, 요청으로 재현한다. 재현하지 못한 의심은 '근거 약함'으로 따로 적는다.

출처: openai/skills security-best-practices, anthropics/claude-plugins-official security-guidance, getsentry/skills find-bugs(Apache-2.0)의 점검 항목과, getsentry/skills security-review, trailofbits/skills sharp-edges, OWASP Node.js 보안 치트시트(CC BY-SA 4.0)의 판정 기준을 참고해 새로 쓴 글이다. 원문은 옮기지 않았다(NOTICE.md 참고).
