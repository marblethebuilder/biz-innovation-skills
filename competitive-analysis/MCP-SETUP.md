# MCP 연동 설정 가이드 — Competitive Analysis

경쟁사 분석 스킬에서 사용하는 4개 MCP 서버의 설치 및 연동 방법을 안내합니다.

---

## 연동된 MCP 서버 목록

| 서버 | 용도 |
|------|------|
| `dart-openapi` | 국내 상장사 공시 조회 (DART 전자공시시스템) |
| `sec-edgar` | 미국 상장사 공시 조회 (SEC EDGAR) |
| `kipris` | 국내 상표·특허 조회 (KIPRIS) |
| `alpha-vantage` | 주가·재무 데이터 조회 (Alpha Vantage) |

---

## 사전 준비

Node.js가 설치되어 있어야 합니다.

```bash
node -v  # v18 이상 권장
```

---

## Step 1. API 키 발급

### DART API 키
1. [DART 오픈 API](https://opendart.fss.or.kr/) 접속
2. 회원가입 → 인증키 신청
3. 발급된 인증키 복사

### SEC EDGAR (API 키 없음, User-Agent만 필요)
- API 키 없이 사용 가능
- 대신 `SEC_USER_AGENT` 값에 **본인 이름과 이메일**을 입력해야 함 (SEC 정책)
- 형식: `홍길동 gildong@email.com`

### KIPRIS API 키
1. [KIPRIS 포털](https://www.kipris.or.kr/portal/main/mainView.do) 접속
2. 회원가입 → 마이페이지 → API 신청
3. 발급된 키 복사

### Alpha Vantage API 키
1. [Alpha Vantage](https://www.alphavantage.co/support/#api-key) 접속
2. 이메일 입력 후 무료 키 발급 (즉시)
3. 발급된 키 복사

---

## Step 2. 이 레포 클론

```bash
git clone https://github.com/marblethebuilder/biz-innovation-skills.git
cd biz-innovation-skills
```

---

## Step 3. mcp.json 파일 생성

`competitive-analysis/mcp.json.example`을 복사해서 사용합니다.

**Mac/Linux:**
```bash
cp competitive-analysis/mcp.json.example ~/.mcp.json
```

**Windows:**
```powershell
Copy-Item competitive-analysis\mcp.json.example $HOME\.mcp.json
```

---

## Step 4. mcp.json 편집

복사한 `~/.mcp.json` 파일을 열어 아래 두 가지를 수정합니다.

### 1) 경로 수정

`/YOUR/LOCAL/PATH/` 부분을 이 레포를 클론한 실제 경로로 변경합니다.

**예시 (Mac):**
```json
"args": ["/Users/홍길동/biz-innovation-skills/competitive-analysis/mcp-servers/dart-openapi/index.js"]
```

**예시 (Windows):**
```json
"args": ["C:\\Users\\홍길동\\biz-innovation-skills\\competitive-analysis\\mcp-servers\\dart-openapi\\index.js"]
```

### 2) API 키 입력

`YOUR_XXX_API_KEY` 자리에 Step 1에서 발급받은 키를 입력합니다.

```json
{
  "mcpServers": {
    "dart-openapi": {
      "command": "node",
      "args": ["/Users/홍길동/biz-innovation-skills/competitive-analysis/mcp-servers/dart-openapi/index.js"],
      "env": {
        "DART_API_KEY": "발급받은_DART_키_입력"
      }
    },
    "sec-edgar": {
      "command": "node",
      "args": ["/Users/홍길동/biz-innovation-skills/competitive-analysis/mcp-servers/sec-edgar/index.js"],
      "env": {
        "SEC_USER_AGENT": "홍길동 gildong@email.com"
      }
    },
    "kipris": {
      "command": "node",
      "args": ["/Users/홍길동/biz-innovation-skills/competitive-analysis/mcp-servers/kipris/index.js"],
      "env": {
        "KIPRIS_API_KEY": "발급받은_KIPRIS_키_입력"
      }
    },
    "alpha-vantage": {
      "command": "node",
      "args": ["/Users/홍길동/biz-innovation-skills/competitive-analysis/mcp-servers/alpha-vantage/index.js"],
      "env": {
        "ALPHA_VANTAGE_API_KEY": "발급받은_AlphaVantage_키_입력"
      }
    }
  }
}
```

---

## Step 5. Claude Code 재시작

```bash
# Claude Code를 완전히 종료 후 재실행
claude
```

재시작 후 `/mcp` 명령어로 연결 상태를 확인합니다.

---

## 주의사항

- `~/.mcp.json` 파일은 **절대 GitHub에 올리지 않습니다.** API 키가 포함되어 있기 때문입니다.
- `.gitignore`에 `.mcp.json`을 추가하는 것을 권장합니다.
- API 키가 외부에 유출되면 즉시 해당 서비스에서 키를 재발급 받습니다.

---

## 문의

설정 중 문제가 생기면 이 레포 Issues에 남겨주세요.
