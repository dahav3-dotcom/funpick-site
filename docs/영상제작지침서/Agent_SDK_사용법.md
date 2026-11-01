# Agent SDK 사용법 (영상 파이프라인 자동화 기준)

## 1. 무엇인가
Claude Code를 라이브러리로 쓰는 것. Python/TypeScript 코드에서 `query()` 한 줄로 Claude Code 엔진(파일 읽기·쓰기, Bash, 웹검색, 서브에이전트, 훅, 세션)을 돌린다. 폴더의 CLAUDE.md·스킬·설정을 Claude Code와 똑같이 읽는다.

| | Claude Code CLI | `claude -p` | Agent SDK | Client SDK(Claude API) |
|---|---|---|---|---|
| 용도 | 대화형 작업 | 셸에서 한 번 실행 | 코드에서 반복·자동 실행 | 앱 안에서 Claude와 대화만 |
| 도구 | 전부 | 전부 | 전부 | 없음(직접 구현) |
| 과금 | 플랜 한도 | 플랜 한도(API 키를 넣어도 월간 API 크레딧 대상 아님) | 월간 API 크레딧 | 월간 API 크레딧 |
| 적합 | 기획·수정 | 단계 하나 수동 실행 | 소재 10개 × 단계 8개 루프, 예산 상한, 훅 | 게임 NPC, 챗봇, 요약 버튼 |

- 게임·앱을 "만드는" 것은 Claude Code로. 앱 "안에" Claude를 넣는 것은 Client SDK(대화) 또는 Agent SDK(파일·명령까지 하는 일꾼).
- 배포 앱은 내 API 키로 과금되며 사용자의 claude.ai 구독을 끌어다 쓰는 방식은 허용되지 않는다. 제품명에 "Claude Code" 사용 불가, "Powered by Claude"는 가능.

## 2. 설치
```bash
python3 -m venv .venv && source .venv/bin/activate && pip install claude-agent-sdk
```
```bash
export ANTHROPIC_API_KEY=sk-ant-...   # 연결된 콘솔 조직의 키 → 월간 API 크레딧에서 차감
```

## 3. 최소 예제: 소재 3개의 리서치 단계를 순서대로 실행
```python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions

소재들 = ["박명수", "이수근", "나영석"]

async def main():
    for 이름 in 소재들:
        options = ClaudeAgentOptions(
            cwd=f"/Users/나/분석영상/{이름}",            # 폴더 루트에 CLAUDE.md
            permission_mode="acceptEdits",                # 파일 쓰기 자동 승인
            allowed_tools=["Read", "Write", "Bash", "WebSearch", "WebFetch"],
            model="claude-opus-5-5",
            max_budget_usd=5.0,                           # 한 소재당 상한(추정치라 여유 두기)
        )
        async for msg in query(
            prompt=f"{이름} 편 1단계: 사실표.md(30행, 출처 URL 필수)와 인용후보.md(본인 발화 20개)를 만들어 줘",
            options=options,
        ):
            if type(msg).__name__ == "ResultMessage":
                print(이름, "완료, 비용 추정:", msg.total_cost_usd)

asyncio.run(main())
```
실행: API 키를 내보낸 터미널에서 `python run.py`.

## 4. 주요 옵션
| 옵션 | 뜻 |
|---|---|
| `cwd` | 작업 폴더. CLAUDE.md가 여기 있어야 규칙이 적용됨 |
| `permission_mode` | default / acceptEdits / plan / dontAsk / bypassPermissions / auto |
| `allowed_tools` | 승인 없이 쓸 도구. 제한이 아니라 자동 승인 목록 |
| `system_prompt` | 문자열 또는 `{"type":"preset","preset":"claude_code","append":"..."}` |
| `max_turns`, `max_budget_usd` | 턴 수·비용 상한 |
| `continue_conversation=True` / `resume` | 이전 세션 이어가기(기본은 매번 새 세션) |

## 5. 비용 감각
Opus 5.5 입력 $4·출력 $20(100만 토큰당). 리서치·대본 단계 한 편 $2~6. 월 $200이면 텍스트 단계는 넉넉. 렌더링·ElevenLabs·Suno는 별도 과금. 사용량은 `ResultMessage.total_cost_usd`(추정)와 콘솔 사용량 페이지에서 확인.

## 6. 문서
- https://code.claude.com/docs/en/agent-sdk/overview
- https://code.claude.com/docs/en/agent-sdk/python
- https://code.claude.com/docs/en/agent-sdk/quickstart
