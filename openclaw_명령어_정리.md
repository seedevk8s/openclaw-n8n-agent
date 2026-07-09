# OpenClaw 명령어 정리 (자주 쓰는 순)

## 1. 매일 쓰는 것

| 명령어 | 설명 |
|---|---|
| `openclaw chat` | 로컬 모드로 바로 대화 시작 (게이트웨이 없이). `terminal`도 같은 별칭 |
| `openclaw tui` | 게이트웨이에 붙어서 TUI로 대화 |
| `openclaw dashboard` | 웹 대시보드(Control UI) 열기 |
| `openclaw status` | 전체 상태 한눈에 확인 (에이전트, 게이트웨이, 모델) |
| `openclaw logs` / `openclaw logs --follow` | 로그 보기 / 실시간 추적 |

### TUI 안에서 자주 쓰는 단축키·슬래시 명령
- `Enter` 전송 / `Esc` 실행 중단 / `Ctrl+D` 종료
- `Ctrl+L` 모델 선택 / `Ctrl+G` 에이전트 선택 / `Ctrl+P` 세션 선택
- `/status`, `/session <key>`, `/model <provider/model>`, `/new`(세션 리셋), `/exit`

## 2. 게이트웨이 관리

| 명령어 | 설명 |
|---|---|
| `openclaw gateway` / `openclaw gateway run` | 게이트웨이 실행 (foreground) |
| `openclaw gateway install` | 서비스로 설치 (launchd/systemd 등) |
| `openclaw gateway start` / `stop` / `restart` | 서비스 시작/중지/재시작 |
| `openclaw gateway restart --safe` | 진행 중 작업 끝나면 안전하게 재시작 |
| `openclaw gateway status [--json] [--deep]` | 게이트웨이 서비스+연결 상태 확인 |
| `openclaw gateway health` | 헬스체크 |
| `openclaw gateway probe` | 로컬/원격 게이트웨이 전부 진단 |
| `openclaw gateway discover` | mDNS로 근처 게이트웨이 찾기 |

## 3. 최초 설정 / 설정 변경

| 명령어 | 설명 |
|---|---|
| `openclaw onboard` | 최초 실행 시 전체 가이드 설정 마법사 (모델 인증, 워크스페이스, 게이트웨이, 채널) |
| `openclaw setup` | 마법사 없이 기본 설정만 빠르게 생성 |
| `openclaw configure` | 기존 설정의 일부(모델, 게이트웨이, 채널 등)만 수정 |
| `openclaw config get/set/unset` | 설정값 직접 조회·변경 |
| `openclaw config validate` | 설정 파일 유효성 검사 |
| `openclaw doctor [--fix]` | 설정/환경 진단 및 자동 복구 |

## 4. 에이전트 / 세션 / 모델

| 명령어 | 설명 |
|---|---|
| `openclaw agents list` | 에이전트 목록 |
| `openclaw agents add <name>` | 새 에이전트 추가 |
| `openclaw sessions` | 세션 관리 (`cleanup` 등) |
| `openclaw models list` / `status` / `set` | 사용 가능 모델 조회·상태·전환 |
| `openclaw message send` | 특정 채널/사람에게 메시지 전송 |

## 5. 자동화

| 명령어 | 설명 |
|---|---|
| `openclaw cron list/add/edit/rm` | 크론(예약 실행) 관리 |
| `openclaw tasks list` | 작업 목록/상태 |
| `openclaw hooks list/enable/disable` | 훅 관리 |

## 6. 채널·연동

| 명령어 | 설명 |
|---|---|
| `openclaw channels add` | 채널 계정 연결 (텔레그램 등) |
| `openclaw channels list/status` | 연결된 채널 확인 |
| `openclaw pairing list/approve` | 기기 페어링 승인 |
| `openclaw plugins list/install/update` | 플러그인 관리 |
| `openclaw skills search/install/list` | 스킬 관리 |

## 7. 백업 / 초기화 / 삭제

| 명령어 | 설명 |
|---|---|
| `openclaw backup create` | 백업 생성 |
| `openclaw reset` | 설정/자격증명/세션 초기화 |
| `openclaw uninstall` | 제거 |
| `openclaw update` | 업데이트 |

## 자주 헷갈리는 것 정리
- `openclaw gateway onboard` ❌ 존재하지 않음 → `openclaw onboard`가 맞음
- `openclaw chat` = `openclaw terminal` = `openclaw tui --local` (전부 동일)
- `/exit`는 TUI 세션 종료, `exit`(슬래시 없이)는 그냥 일반 메시지로 전송됨

---
참고: [CLI reference](https://docs.openclaw.ai/cli) · [TUI](https://docs.openclaw.ai/web/tui) · [Gateway](https://docs.openclaw.ai/cli/gateway) · [Onboard](https://docs.openclaw.ai/cli/onboard)
