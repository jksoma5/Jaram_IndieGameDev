# Unity MCP 연결 가이드 (한 장 요약)

> **목표**: AI(Claude Code 등)가 내 Unity 에디터를 직접 보고 조작할 수 있게 연결하기
> **구조**: `AI 클라이언트(Claude Code)` ⇄ `MCP 서버(unity mcp)` ⇄ `Unity 에디터(Pipeline 패키지)`
> **소요 시간**: 약 10분 · ⚠️ Unity CLI는 현재 **베타**입니다. 화면·명령이 다르면 강사에게 알려주세요.

---

### ✅ 0. 시작 전 체크
- [ ] **Unity 6 이상** 프로젝트가 있고, Play 버튼으로 실행된다 (8번 실습 완료)
- [ ] **Claude Code**가 설치·로그인되어 있다 (다른 AI 클라이언트도 가능: Cursor, VS Code 등)
- [ ] GitHub Desktop에서 **지금 상태를 커밋**했다 ← AI가 프로젝트를 바꾸기 전 세이브 포인트
- [ ] 프로젝트 폴더 경로를 알고 있다 (예: `C:\Users\나\MyGame`, `/Users/나/MyGame`)

### 1. Unity CLI 설치 (최초 1회)

**Windows** — PowerShell 열고:
```powershell
$env:UNITY_CLI_CHANNEL='beta'; irm https://public-cdn.cloud.unity3d.com/hub/prod/cli/install.ps1 | iex
```
**macOS** — 터미널 열고:
```bash
curl -fsSL https://public-cdn.cloud.unity3d.com/hub/prod/cli/install.sh | UNITY_CLI_CHANNEL=beta bash
```
→ **터미널을 닫고 새로 연 뒤** 확인: `unity --version` (버전이 나오면 성공)

### 2. 프로젝트에 연결용 패키지 설치 (프로젝트마다 1회)
```bash
unity pipeline install --project-path "<내 프로젝트 경로>"
```
→ Unity 에디터로 돌아가 패키지 가져오기(import)가 끝날 때까지 기다리기
→ 확인: `unity status` → 내 프로젝트가 **ready** 로 보이면 성공

### 3. AI 클라이언트에 MCP 서버 등록 (최초 1회)
```bash
unity mcp configure claude-code --project-path "<내 프로젝트 경로>"
```
- 다른 도구를 쓰면 `claude-code` 대신 `cursor`, `vscode`, `claude`(Claude Desktop) 등
- 지원 목록 보기: `unity mcp configure --list`

### 4. 연결 확인
1. **Unity 에디터를 켜 둔 상태**에서 터미널로 프로젝트 폴더에 이동 → `claude` 실행
2. `/mcp` 입력 → **unity** 서버가 연결됨(connected)으로 보이는지 확인
3. AI에게 요청: **"현재 씬의 Hierarchy를 알려줘"**
   → 내 씬의 오브젝트 이름(Main Camera 등)이 나오면 🎉 **연결 완료**

### 5. 첫 요청 해보기
> "씬에 Player라는 큐브를 만들고, WASD로 움직이는 PlayerController 스크립트를 Assets/Scripts에 만들어서 붙여줘. 다른 오브젝트는 건드리지 마."

→ Unity에서 **Play로 직접 확인** → 잘 되면 GitHub Desktop에서 **변경 파일 확인 후 커밋**

---

### 🛠 막혔을 때

| 증상 | 해결 |
|---|---|
| `unity` 명령을 찾을 수 없음 | 터미널을 **새로 열기**. 그래도 안 되면 1단계 다시 실행 |
| `unity status`에 프로젝트가 안 보임 / 연결 시간 초과 | Unity 에디터가 켜져 있는지 확인. **Console에 빨간 컴파일 에러가 있으면 Safe Mode**라 연결이 안 됨 → 에러부터 고치고 Unity 재시작 |
| `AMBIGUOUS_EDITOR` 에러 | Unity 에디터가 여러 개 열림 → 다른 프로젝트 닫거나 `--project-path`를 정확히 지정 |
| `/mcp`에 unity가 없음 | Claude Code를 종료 후 다시 실행. 3단계를 다시 실행 |
| AI가 엉뚱한 프로젝트를 수정 | 즉시 중단 → GitHub Desktop에서 **Discard changes**로 되돌리기 |

### ⚠️ MCP 안전 수칙 (꼭 지키기)
1. **AI 작업 전에 커밋** — 망가져도 되돌릴 수 있다
2. **한 번에 기능 하나만** 요청한다
3. 파일 삭제·대량 변경은 **내용을 읽고 승인**한다
4. 결과는 **반드시 Play로 직접 확인**한다
5. 이해 못 한 코드는 "이 코드가 뭘 하는지 설명해줘" 후 커밋한다

---
<sub>강사용 메모: Unity CLI 베타 기준(`unity mcp`, `unity pipeline`, `unity mcp configure`)으로 작성. 수업 직전 Windows/Mac 각각 1회 리허설 필수. 대안(Plan B)으로 오픈소스 Unity MCP 패키지(GitHub 커뮤니티 프로젝트)를 준비해 두되, 설치 URL·요구 런타임(Python 등)은 최신 README로 확인할 것.</sub>
