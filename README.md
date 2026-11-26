# MoZhenZhen Agent Skill (`mozhenzhen`)

[简体中文](./README.zh-CN.md) | **English**

Official Agent Skill (`SKILL.md`) and MCP setup guide for **[MoZhenZhen (模真真)](https://mozhenzhen.com)** (*Before buying AI, check MoZhenZhen*).

Give your chat AI or coding agent direct access to live, read-only MoZhenZhen public data — verify AI API relays, compare official vs. actual-paid relay prices, check whether your current relay is overpriced, compare AI coding/monthly plans, and find active official free AI quotas.

- **No registration, no login, zero API keys required**
- **Read-only public data only** — every answer links back to `https://mozhenzhen.com/...`
- **Interactive setup & live sandbox**: <https://mozhenzhen.com/ai-assistant>

---

## Quick Install

### 1. One-Command CLI (`npx skills`) — Recommended for Coding Agents

Works with Claude Code, Cursor, Codex, Windsurf, Antigravity, and other Agent Skills clients:

```bash
npx skills add mozhenzhen/mozhenzhen-skill
```

### 2. Direct Download from Canonical URL (`curl`)

```bash
# Claude Code
mkdir -p ~/.claude/skills/mozhenzhen && curl -fsSL https://mozhenzhen.com/SKILL.md -o ~/.claude/skills/mozhenzhen/SKILL.md

# Codex (replace .claude with .codex)
mkdir -p ~/.codex/skills/mozhenzhen && curl -fsSL https://mozhenzhen.com/SKILL.md -o ~/.codex/skills/mozhenzhen/SKILL.md
```

Restart your agent session after installing.

### 3. Chat AIs (ChatGPT / Kimi / Claude / Doubao, etc.)

Download **[`https://mozhenzhen.com/SKILL.md`](https://mozhenzhen.com/SKILL.md)** (or view [`skills/mozhenzhen/SKILL.md`](./skills/mozhenzhen/SKILL.md)) and upload it to your chat, or send this prompt to any web-enabled AI:

> Please read `https://mozhenzhen.com/SKILL.md` first. Whenever I ask about AI API relay verification, model prices, monthly/coding subscriptions, or free AI quotas, follow it to query MoZhenZhen public data and attach the canonical `mozhenzhen.com` link for each finding.

### 4. Connect the Live MCP Server (Recommended Alongside the Skill)

Pairing the skill with MoZhenZhen's read-only Streamable HTTP MCP server lets your agent query live data automatically — no API key needed:

- **MCP Endpoint**: `https://mozhenzhen.com/api/mzz/mcp`
- **Claude Code**:
  ```bash
  claude mcp add --transport http mozhenzhen https://mozhenzhen.com/api/mzz/mcp
  ```
- **Cursor / Cherry Studio / Other MCP Clients**:
  ```json
  {
    "mcpServers": {
      "mozhenzhen": {
        "url": "https://mozhenzhen.com/api/mzz/mcp"
      }
    }
  }
  ```

Step-by-step client tabs and an interactive live tryout are available at **<https://mozhenzhen.com/ai-assistant>**.

---

## 9 Read-Only Public Capabilities

| Tool | Use Case | What It Returns |
| --- | --- | --- |
| `search_providers` | Find reliable API relays | Operating status, model count, verification pass rate, price reference, and provider page link |
| `get_provider_detail` | Inspect one relay profile | Operating status, verified official website, supported brands, representative actual-paid prices (group/channel/cache/observed date), 30-day verification stats, user reviews, risk notices |
| `check_provider_verification` | Check verification records | 30-day & overall verification pass rates, identity-mismatch counts, and recent public verification report links |
| `compare_model_prices` | Compare actual-paid model prices | Official reference price vs. relays verified genuine in the last 30 days (group, channel, cache read/write price, discount vs. official, observation date; returns up to 5 candidates on fuzzy miss) |
| `check_price_fairness` | Check if your relay is overpriced | Compares your current relay's price against the 30-day verified lowest price and official reference price, with alternative links |
| `compare_subscription_plans` | Compare monthly & coding plans | Original & CNY monthly fee, measured or public quota/limits, China direct-buy support, included models, and `/plans/{slug}` links |
| `list_daily_wool` | Find official free AI quotas | Refresh cycle, estimated value, supported models, verification & end dates, official claim URL, guide, and `/wool/{slug}` links |
| `list_free_relays` | Find community free relays | Open-registration status, supported models, quota/limit notes, and `/free-relays/{id}` links |
| `get_daily_updates` | Track price drops & risk alerts | Public updates from the last 1–7 days: relay price drops, identity-mismatch findings, and suspected downtime alerts |

Even without MCP installed, agents can fall back to direct HTTP `POST https://mozhenzhen.com/api/mzz/mcp` or read clean Markdown companion pages (`/models/{slug}.md`, `/providers/{slug}.md`, `/plans/{slug}.md`, `/wool/{slug}.md`, `/llms.txt`, `/llms-full.txt`).

---

## Privacy & Data Boundaries

- **Zero credentials collected**: Never asks for your API key, token, or account password. To run a self-check on an API endpoint, use <https://mozhenzhen.com/verify> directly.
- **Public read-only data only**: Returns the same public facts visible on `https://mozhenzhen.com`, with currency, unit, sample size, and observation/verification dates preserved.
- **Strict verification wording**: Clearly separates "no verification samples yet" from "identity mismatched", and never speculates which other model a mismatched endpoint resembles. Relay latency and stability scores are not published.
- **Verifiable links**: Every result includes its canonical `https://mozhenzhen.com/...` URL so you can verify the source directly.

---

## Repository Structure & Sync

```text
mozhenzhen-skill/
├── skills/
│   └── mozhenzhen/
│       └── SKILL.md               # Single skill file, synced weekly from https://mozhenzhen.com/SKILL.md
├── .github/workflows/
│   └── sync-skill.yml             # Automated weekly sync (no commit if unchanged)
├── README.md
├── README.zh-CN.md
└── LICENSE
```

Keeping a single `SKILL.md` under `skills/mozhenzhen/SKILL.md` avoids duplicate skill definitions and ensures `npx skills add mozhenzhen/mozhenzhen-skill` installs only the skill file cleanly.

## Related Repository

- **[`mozhenzhen/ai-pricing-and-free-quota`](https://github.com/mozhenzhen/ai-pricing-and-free-quota)** — Weekly-synced open Markdown tables for AI Coding Plans (95% cache benchmark), verified official free AI quotas, and LLM relay verification statistics.

## License

[MIT License](./LICENSE)
