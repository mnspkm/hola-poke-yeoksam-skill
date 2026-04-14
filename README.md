# 🐟 올라포케 역삼 · AI 스킬

대한민국 1호 에이전틱 포케집. **AI 에이전트에게 "역삼 올라포케 응모해줘"라고 시키면 에이전트가 당신의 번호로 이벤트에 응모합니다.** 5% 확률로 15,000원 적립 당첨, 95% 확률로 새우 토핑 위로상.

원격 MCP 서버에 연결되는 Skill. Claude Desktop · Cursor · Claude Code 등에서 30초 안에 연결.

> 💡 서버 백엔드 소스는 공개하지 않습니다. 이 레포는 **Skill 정의 + 설치 가이드**만 담고 있어요. 영감을 준 [JinGuYuan/jinguyuan-dumpling-skill](https://github.com/JinGuYuan/jinguyuan-dumpling-skill)의 구조를 따릅니다.

## 30초 설치 — AI에게 시키기 (Claude Code / Cursor)

제일 쉬운 방법. AI에게 한 줄:

> "이 repo로 올라포케 스킬 설치해줘: https://github.com/mnspkm/hola-poke-yeoksam-skill"

에이전트가 알아서 `~/.claude/skills/` 또는 `~/.cursor/skills/`에 복제합니다. 그 다음 바로 대화:

> "올라포케 역삼 이벤트 응모해줘"

## 수동 설치

<details>
<summary><b>Claude Desktop</b> (stdio 전용 → <code>mcp-remote</code> 프록시 필요)</summary>

`~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) 또는 `%APPDATA%\Claude\claude_desktop_config.json` (Windows)에 추가:

```json
{
  "mcpServers": {
    "hola-poke-yeoksam": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://hola-poke-yeoksam-skill.onrender.com/mcp"]
    }
  }
}
```

Node.js 필요 (`npx`). Claude Desktop 재시작하면 🔌 아이콘에 `hola-poke-yeoksam`이 보입니다.

</details>

<details>
<summary><b>Cursor</b> (HTTP MCP 네이티브 지원)</summary>

`~/.cursor/mcp.json`에:

```json
{
  "mcpServers": {
    "hola-poke-yeoksam": {
      "url": "https://hola-poke-yeoksam-skill.onrender.com/mcp"
    }
  }
}
```

</details>

<details>
<summary><b>Claude Code</b> (SKILL.md 인식 + Cursor 축약 형식)</summary>

레포를 skills 폴더에 복제:

```bash
git clone https://github.com/mnspkm/hola-poke-yeoksam-skill \
  ~/.claude/skills/hola-poke-yeoksam-skill
```

</details>

## 뭘 물어볼 수 있어?

| 프롬프트 | 호출되는 도구 |
|---|---|
| "올라포케 메뉴 뭐 있어?" | `get_menu` |
| "역삼점 어디야? 영업시간은?" | `get_shop_info` |
| "단체주문 하고 싶어" | `get_shop_info` (네이버 단체주문 URL 포함) |
| "올라포케 이벤트 응모해줘" | `enter_raffle` |

실제 대화 흐름:

> **나**: 올라포케 이벤트 응모하고 싶어
>
> **AI**: 휴대폰 번호 알려주세요 (응모 결과 대조용으로만 써요, 별도 마케팅 발송 없음).
>
> **나**: 010-1234-5678
>
> **AI** (95% 경우):
> 이번엔 아쉽지만 AI가 새우 토핑 하나 챙겨드렸어요 🦐
>
> 코드: `Claw-A3K9`
>
> 배달앱 가게메모에 이 코드 넣으시면 새우 토핑 서비스예요.

5% 걸리면:

> **AI**: 🎉 5% 뚫었어요! 총 15,000원 적립 당첨
>
> 코드: `Jackpot-A3K9`
>
> 매장 방문하셔서 "이 코드 당첨됐어요" 말씀 주시면 사장님이 번호 받아서 적립 넣어드려요. (3회 나눠 사용 — 방문 때마다 2,500 / 5,000 / 7,500)

## 응모 규칙

- **확률**: 5% 당첨 (`Jackpot-XXXX`) / 95% 위로상 (`Claw-XXXX`)
- **상환**:
  - Jackpot: **매장 방문 시** 번호 + 코드 제시 → POS 수기 적립 (3회 분할)
  - Claw: **배달앱 가게메모**에 코드 입력 → 새우 토핑 서비스 (매장도 가능)
- **검증**: 매장 상주 사장이 번호 + 코드 페어로 대면 확인
- **번호**: 대조용만. 별도 마케팅 발송·3자 공유 없음

## 라이선스

MIT. Skill 정의·설치 가이드 포크·리믹스 환영 — 다른 가게에서 응용해도 좋아요.

---

**Made with ❤️ in 역삼 · by [@mnspkm](https://github.com/mnspkm)**

_영감: [JinGuYuan/jinguyuan-dumpling-skill](https://github.com/JinGuYuan/jinguyuan-dumpling-skill) — 北邮 옆 20년 饺子馆의 공식 AI 스킬._
