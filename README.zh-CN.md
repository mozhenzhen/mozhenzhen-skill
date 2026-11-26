# 模真真 AI 助手技能（`mozhenzhen`）

**简体中文** | [English](./README.md)

**[模真真（mozhenzhen.com）](https://mozhenzhen.com)** 官方维护的通用 Agent Skill（`SKILL.md`）与 MCP 接入仓库（*买 AI 前，先查模真真*）。

装好后，你的聊天 AI 或编程助手就能直接查模真真公网公开数据：核对中转站真假验真与避雷、比模型官方价与中转实付价、测当前中转站贵不贵、挑包月与编程套餐、查每日官方免费额度与公益站。

- **免注册、免登录、绝不索要 API Key**
- **只读公开数据**，每条结论自带 `https://mozhenzhen.com/...` 原页链接可核对
- **在线体验与图形化接入引导**：<https://mozhenzhen.com/ai-assistant>

---

## 快速安装

### 1. 命令行一键安装（`npx skills`）· 推荐编程助手使用

适用于 Claude Code、Cursor、Codex、Windsurf、Antigravity 等支持 Agent Skills 的工具：

```bash
npx skills add mozhenzhen/mozhenzhen-skill
```

### 2. 从官网唯一事实源直接下载（`curl`）

```bash
# Claude Code
mkdir -p ~/.claude/skills/mozhenzhen && curl -fsSL https://mozhenzhen.com/SKILL.md -o ~/.claude/skills/mozhenzhen/SKILL.md

# Codex（将路径里的 .claude 换成 .codex）
mkdir -p ~/.codex/skills/mozhenzhen && curl -fsSL https://mozhenzhen.com/SKILL.md -o ~/.codex/skills/mozhenzhen/SKILL.md
```

安装完成后重开一次对话即可生效。

### 3. 日常聊天 AI（ChatGPT / Kimi / 豆包 / Claude 等）

下载 **[`https://mozhenzhen.com/SKILL.md`](https://mozhenzhen.com/SKILL.md)**（或直接查看本仓 [`skills/mozhenzhen/SKILL.md`](./skills/mozhenzhen/SKILL.md)）发进聊天框，或者把下面这句话发给支持联网的 AI：

> 请先阅读 `https://mozhenzhen.com/SKILL.md` 里的使用说明，以后我问 AI 中转站真假、模型报价、包月套餐或免费 AI 额度时，按这份说明帮我查模真真公开数据，并在每条结论后附上官网原页链接。

### 4. 配置 MCP 实时直连（推荐与 Skill 搭配使用）

配置一次后，聊到中转站、模型价格或免费额度时 AI 会自动调用只读公开接口（免密钥）：

- **MCP 地址（Streamable HTTP）**：`https://mozhenzhen.com/api/mzz/mcp`
- **Claude Code 一行接入**：
  ```bash
  claude mcp add --transport http mozhenzhen https://mozhenzhen.com/api/mzz/mcp
  ```
- **Cursor / Cherry Studio / 其他 MCP 客户端**：
  ```json
  {
    "mcpServers": {
      "mozhenzhen": {
        "url": "https://mozhenzhen.com/api/mzz/mcp"
      }
    }
  }
  ```

更多客户端步骤与免安装在线试跑请访问 **<https://mozhenzhen.com/ai-assistant>**。

---

## 9 项公开只读能力速查

| 工具名 | 适用场景 | 返回要点 |
| --- | --- | --- |
| `search_providers` | 找靠谱中转站 | 按关键词、品牌、运营状态或最低验真通过率筛选，返回状态、模型数、验真通过率与主页链接 |
| `get_provider_detail` | 查单站档案 | 运营状态、已核实官网、品牌、代表模型实付报价（分组/渠道/缓存价/观察日）、近 30 天验真、口碑与风险提示 |
| `check_provider_verification` | 查验真记录 | 近 30 天及累计验真通过率、身份不符次数与最近公开验真报告链接 |
| `compare_model_prices` | 比模型实付价 | 官方参考价对比近 30 天验过真中转站实付价（分组、渠道、缓存读写价、折合几折、观察日；模糊未精中返最多 5 个候选） |
| `check_price_fairness` | 测我这家贵不贵 | 对比你正在用的中转站现价、「近 30 天验过真最低价」和「官方参考价」，给直观结论与替代参考链接 |
| `compare_subscription_plans` | 包月与编程套餐比价 | 月费原价与折算人民币（¥/月）、实测或公开参考额度/限额、是否支持国内直购、适用模型与 `/plans/{slug}` 链接 |
| `list_daily_wool` | 领每日免费额度 | 刷新周期、预估价值、可用模型、核实与截止日期、官方领取入口、教程与 `/wool/{slug}` 链接 |
| `list_free_relays` | 找免费公益站 | 开放注册状态、可用模型、额度与限制说明及 `/free-relays/{id}` 链接 |
| `get_daily_updates` | 看最近降价与避雷动态 | 近 1–7 天公开变动：中转站降价、验出身份不符、停运预警 |

未安装 MCP 时，AI 也可直接向 `https://mozhenzhen.com/api/mzz/mcp` 发送 JSON-RPC HTTP 请求，或读取纯文本 Markdown 伴生页（`/models/{slug}.md`、`/providers/{slug}.md`、`/plans/{slug}.md`、`/wool/{slug}.md`、`/llms.txt`、`/llms-full.txt`）。

---

## 隐私与公开口径说明

- **绝不索要凭据**：全程不需要登录账号，也不索要 API Key、Token 或密码；如需自测手头的中转站接口，请直接前往 <https://mozhenzhen.com/verify>。
- **只读公开事实**：数据与模真真公网页面一致，保留币种、计价单位、样本量与观察/核实时间，不编造未返回字段。
- **严谨区分验真结论**：明确区分「暂无验真样本」与「身份不符」，验出不符时不臆测「更像某型号」；不公开展示中转站时延或稳定性评分，买号商品不点名店铺横评。
- **每条都能核对**：回答附带 `https://mozhenzhen.com/...` 原页链接，方便随时点开复核。

---

## 仓库结构与自动同步

```text
mozhenzhen-skill/
├── skills/
│   └── mozhenzhen/
│       └── SKILL.md               # 唯一技能文件，每周自动从 https://mozhenzhen.com/SKILL.md 同步
├── .github/workflows/
│   └── sync-skill.yml             # 每周自动同步工作流（内容无变化则不提交）
├── README.md
├── README.zh-CN.md
└── LICENSE
```

仓库仅在标准路径 `skills/mozhenzhen/SKILL.md` 维护唯一一份技能文件，避免与根目录重复定义冲突，并保证 `npx skills add mozhenzhen/mozhenzhen-skill` 安装时只写入干净的技能目录。

## 关联公开仓库

- **[`mozhenzhen/ai-pricing-and-free-quota`](https://github.com/mozhenzhen/ai-pricing-and-free-quota)** — 每周自动同步的 AI 编程套餐比价（95% 缓存场景）、官方免费 AI 额度清单与中转站公开验真统计 Markdown 数据表。

## 开源许可

[MIT License](./LICENSE)
