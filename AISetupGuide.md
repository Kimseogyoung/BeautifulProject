# 🧠 AI Dev Environment Setup Guide (Windows)

이 문서는 Windows 환경에서\
Codex CLI + Claude Code + Serena + Context7\
로컬 MCP 구성을 빠르게 재현하기 위한 가이드입니다.

------------------------------------------------------------------------

# 1️⃣ 기본 환경 세팅

## Python (python.org 버전 권장)

### 설치 (CMD)

    winget install Python.Python.3.11

설치 후 확인:

    python --version
    where.exe python

## uv 설치

    python -m pip install --upgrade pip
    python -m pip install uv

확인:

    uv --version
    uvx --version
    where.exe uvx

------------------------------------------------------------------------

## Node.js LTS 설치

    winget install OpenJS.NodeJS.LTS

확인:

    node --version
    npm --version
    where.exe npx

### PowerShell npm 오류 해결

    Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned

------------------------------------------------------------------------

# 2️⃣ Codex CLI 설치

    npm install -g @openai/codex

확인:

    codex --version

------------------------------------------------------------------------

# 3️⃣ Codex MCP 설정 (전역)

파일 위치: C:`\Users`{=tex}\<USER\>.codex`\config`{=tex}.toml

내용:

``` toml
[mcp_servers.serena]
type = "stdio"
command = "uvx"
args = ["--from", "git+https://github.com/oraios/serena", "serena", "start-mcp-server", "--context=ide", "--project-from-cwd"]

[mcp_servers.context7]
type = "stdio"
command = "cmd"
args = ["/c", "npx", "-y", "@upstash/context7-mcp@latest"]
```

확인:

    codex mcp list

------------------------------------------------------------------------

# 4️⃣ Claude Code CLI 설치

    npm install -g @anthropic-ai/claude-code

확인:

    claude --version

------------------------------------------------------------------------

# 5️⃣ Claude MCP 설정 (Windows 로컬 안정 버전)

⚠ Windows에서는 npx 직접 실행 시 연결 실패 가능\
→ 반드시 cmd /c 사용

## Serena 추가 (프로젝트 절대경로 권장)

    claude mcp add --transport stdio serena --scope user -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context=claude-code --project "<PROJECT_ROOT>"

예:

    --project "D:\work\myproject"

## Context7 추가

    claude mcp add --transport stdio context7 --scope user -- cmd /c npx -y @upstash/context7-mcp@latest

------------------------------------------------------------------------

# 6️⃣ 문제 해결 체크리스트

## npm 실행 오류

    Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned

## where 명령이 안 나올 때 (PowerShell)

    where.exe python
    where.exe uvx

## MCP Failed to connect

-   Claude 재시작
-   PROJECT 경로 절대경로 사용
-   cmd /c npx 사용 여부 확인

------------------------------------------------------------------------

# 7️⃣ 권장 운영 방식

  도구       역할
  ---------- ----------------------------
  Serena     코드 탐색 / 심볼 분석
  Context7   문서 검색 / 라이브러리 RAG
  Codex      코드 수정 중심
  Claude     설계 / 리뷰 중심

------------------------------------------------------------------------

# ✅ 신규 프로젝트 생성 시 체크리스트

-   [ ] Python 설치 확인
-   [ ] uv 설치 확인
-   [ ] Node 설치 확인
-   [ ] Codex mcp list 정상
-   [ ] Claude mcp list 정상
-   [ ] Serena 프로젝트 경로 맞는지 확인

------------------------------------------------------------------------

끝.
