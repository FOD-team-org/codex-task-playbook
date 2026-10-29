---
name: http-roundtrip-test
description: Express 앱의 API 테스트를 만들 때 사용한다. 서버를 빈 포트에 띄워 실제 HTTP 요청으로 확인하는 node:test 틀과, 라우트마다 넣을 오류 사례를 정한다.
---

# HTTP 왕복 테스트

1. 앱 파일(예: server.js)은 app을 만드는 함수를 내보내고, 직접 실행될 때만 listen한다.
```js
const express = require('express');
function createApp(options = {}) {
  const app = express();
  // 라우트
  return app;
}
if (require.main === module) createApp().listen(process.env.PORT || 3000, '0.0.0.0');
module.exports = { createApp };
```
2. 테스트(예: test/api.test.js)는 빈 포트(0)에 띄우고 끝나면 닫는다. require 경로는 실제 파일 위치에 맞춘다.
```js
const { test, before, after } = require('node:test');
const assert = require('node:assert/strict');
const { createApp } = require('../server');
let server, base;
before(async () => {
  server = createApp().listen(0, '127.0.0.1');
  await new Promise((resolve) => server.once('listening', resolve));
  base = `http://127.0.0.1:${server.address().port}`;
});
after(() => server && new Promise((resolve) => server.close(resolve)));
test('목록 200', async () => {
  const res = await fetch(`${base}/api/items`);
  assert.equal(res.status, 200);
  assert.match(res.headers.get('content-type'), /json/);
});
```
3. 데이터 폴더, 기준 시각, 관리자 토큰처럼 바뀌는 값은 createApp 인자나 환경 변수로 받는다. 테스트는 작은 고정 데이터로 돌린다. 파일에 쓰는 테스트는 os.tmpdir() 아래 사본 폴더를 쓰고, 원본 data/는 바꾸지 않는다.
4. 기대값은 요구사항과 손 계산으로 정한다. 코드를 돌려 나온 값을 기대값으로 붙여 넣지 않는다.
5. 라우트마다 정상 1건에 더해 해당하는 사례를 넣는다.
   - 잘못된 형식 400(본문 모양까지 확인)
   - 없는 대상 404
   - 배열로 온 query(`?q=a&q=b`)
   - 깨진 JSON 본문 400, 응답에 스택 없음
   - 권한 없음 401, 비밀값 미설정 시 경로 닫힘
6. 고정 포트, sleep 대기, 외부 네트워크를 쓰지 않는다. package.json의 test는 `node --test`다. 샌드박스에서 spawn EPERM으로 실패하면 `--test-isolation=none`을 붙인다.
7. npm test가 끝났는데 프로세스가 남으면 닫지 않은 서버나 타이머를 찾는다.

출처: anthropics/skills webapp-testing(Apache-2.0)의 "서버를 띄우고 기다린 뒤 확인하고 정리한다"는 흐름을 참고해, 브라우저 없이 Node 24.18에서 직접 실행해 확인한 틀로 새로 쓴 글이다.
