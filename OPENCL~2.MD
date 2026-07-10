# OpenClaw 게이트웨이에 새 테스트 에이전트 추가하기

같은 게이트웨이(같은 포트, 같은 디스코드 봇)를 쓰면서, 특정 디스코드 채널에서만 응답하는 별도 에이전트를 만드는 절차입니다. 실제로 `openclaw002`에 `sandbox` 에이전트를 추가하며 검증한 순서입니다.

## 핵심 개념 (먼저 이해할 것)

- **포트/봇 토큰은 공유됨**: 새 에이전트를 추가해도 새 포트나 새 디스코드 봇이 필요 없습니다. 하나의 게이트웨이 프로세스 안에서 워크스페이스만 분리됩니다.
- **`bindings`와 `채널 허용목록`은 별개**: 이게 제일 헷갈리는 부분입니다.
  - `bindings` = 메시지가 들어왔을 때 **어느 에이전트**로 보낼지 결정
  - `channels.discord.guilds.<길드ID>.channels.<채널ID>` = 그 채널 메시지를 **애초에 받을지 말지** 결정 (`groupPolicy: allowlist`인 경우 필수)
  - 이 둘 중 하나만 하면 안 됩니다. **둘 다** 해줘야 봇이 응답합니다.

## 절차

### 1. 현재 상태 확인
```bash
openclaw agents list --bindings
openclaw config validate
```

### 2. 새 에이전트 워크스페이스 생성
```bash
openclaw agents add <에이전트ID> --workspace ~/.openclaw/workspace-<에이전트ID> --non-interactive
```

### 3. 페르소나 파일 작성
`~/.openclaw/workspace-<에이전트ID>/` 아래에 4개 파일:
- `IDENTITY.md` — 표시 이름 (예: `이름: 🧪 샌드박스`)
- `SOUL.md` — 성격/말투
- `USER.md` — 이 에이전트가 모시는 사람이 누구인지
- `AGENTS.md` — 담당 업무 규칙, 하지 말아야 할 것 (특히 "다른 에이전트 세션/파일 건드리지 않기" 명시)

### 4. 테스트용 디스코드 채널 ID + 서버(길드) ID 확보
- 디스코드 설정 → 고급 → 개발자 모드 ON
- 채널 우클릭 → ID 복사 (채널 ID)
- 서버 아이콘 우클릭 → ID 복사 (길드 ID, 기존 채널과 같은 서버면 생략 가능)

### 5. 라우팅 바인딩 추가
```bash
openclaw config get bindings   # 기존 바인딩 있는지 먼저 확인
openclaw config set bindings '[{"agentId":"<에이전트ID>","match":{"channel":"discord","peer":{"kind":"channel","id":"<채널ID>"}}}]'
```
기존 바인딩이 이미 있었다면 배열에 항목을 추가하는 식으로 합쳐야 함 (통째로 덮어쓰면 기존 규칙 날아감).

### 6. 디스코드 채널 허용목록에 추가 (★ 빠뜨리기 쉬운 단계)
```bash
openclaw config get channels.discord   # groupPolicy가 allowlist인지 먼저 확인
openclaw config set channels.discord.guilds.<길드ID>.channels.<채널ID> '{"enabled":true}'
```

### 7. 적용 및 검증
```bash
openclaw config validate
openclaw gateway restart
openclaw agents list --bindings
```
`agents list` 결과에서 새 에이전트가 `Routing rules: 1`로 뜨는지 확인.

### 8. 실제 테스트
그 디스코드 채널에서 봇을 멘션해서 메시지 전송. 응답 없으면:
```bash
openclaw logs --follow
```
켜둔 채로 재전송 → `discord channels resolved: ...` 줄에 새 채널 ID가 포함돼 있는지 확인.

## 흔한 실수 체크리스트

- [ ] `bindings`만 추가하고 채널 허용목록은 안 건드림 → 조용히 무시됨 (에러도 안 뜸)
- [ ] 새 채널이 기존과 다른 서버(길드)인데 기존 길드ID로 잘못 등록함
- [ ] `openclaw config set bindings`로 기존 바인딩을 통째로 덮어써버림
- [ ] 재시작 안 하고 테스트함 (`config set`은 즉시 반영 안 될 수 있음, 특히 bindings 이외 변경은 재시작 필요)
