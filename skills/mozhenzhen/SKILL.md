---
name: mozhenzhen
description: 买 AI 服务前查模真真（mozhenzhen.com）：查中转站真假验真与避雷、比模型官方价与中转实付价、测当前中转站贵不贵、挑包月与编程套餐、查每日免费额度与公益站。Use when verifying an AI API relay/proxy (中转站), checking verification pass rates or downtime/scam alerts, comparing official vs relay model prices, checking price fairness, comparing AI subscriptions, or finding free AI credits and public relays on 模真真.
---

# 模真真 AI 助手技能

模真真（`https://mozhenzhen.com`）是买 AI 服务前查价、看验真、找免费额度的公开平台。本技能只读公开数据，免注册、免登录、不收 API Key；客户端一键接入见 `https://mozhenzhen.com/ai-assistant`。

## 方式一：优先使用 `mozhenzhen` MCP

- 端点：`https://mozhenzhen.com/api/mzz/mcp`（Streamable HTTP，免密钥、只读从库）
- 协议：`2025-06-18` / `2025-03-26` / `2024-11-05`；`GET` 返回 JSON 工具清单；JSON-RPC batch 单次 ≤ 3 条；字符串参数 ≤ 100 字。
- 返回：`result.content[0].text` 为可读中文汇总（带原始链接），`result.structuredContent` 为结构化 JSON。

### 9 个只读工具速查

| 工具名 | 场景 | 关键入参 | 返回要点 |
| --- | --- | --- | --- |
| `search_providers` | 找靠谱中转站 | `keyword?`、`brand?`（`openai`/`anthropic`/`google`/`deepseek`）、`operating?`（`active`/`inactive`/`classifying`）、`min_pass_rate?`（0–100）、`sort?`（`recommended`/`verification`/`price`/`models`）、`limit?`（1–20，默认 8） | 站点列表、运营状态、模型数、验真通过率、价格参考与主页链接 |
| `get_provider_detail` | 查单站档案 | `query`（必填，站名/slug/域名）、`price_limit?`（1–15，默认 6） | 运营状态、已核实官网、品牌、模型数、代表模型实付报价（分组/渠道/缓存价/观察日）、近 30 天验真、点评均分与风险提示 |
| `check_provider_verification` | 查验真记录 | `query`（必填，站名/slug/域名） | 验真通过率、不符次数与最近公开验真报告链接 |
| `compare_model_prices` | 比模型实付价 | `model`（必填，模型名/slug/model_code）、`limit?`（1–15，默认 8） | 官方参考价、近 30 天验过真中转站实付价（分组、渠道、缓存读写价、折合几折、观察日）；模糊未精中返最多 5 个 `candidates` |
| `check_price_fairness` | 测我这家贵不贵 | `provider`（必填）、`model`（必填） | 该站现价对比「近 30 天验过真最低价」和「官方价」的结论与替代链接 |
| `compare_subscription_plans` | 包月/编程套餐比价 | `keyword?`、`domestic_only?`（默认 `false`）、`limit?`（1–20，默认 10） | 月费原价与折算人民币（¥/月）、实测或公开参考额度/限额、国内直购、适用模型与 `/plans/{slug}` 链接 |
| `list_daily_wool` | 领每日免费额度 | `cycle?`（`day`/`month`/`new`/`limited`）、`api_only?`（默认 `false`）、`limit?`（1–30，默认 12） | 周期、预估价值、可用模型、核实与截止日、官方领取入口、教程与 `/wool/{slug}` 链接 |
| `list_free_relays` | 找免费公益站 | `keyword?`、`open_only?`（默认 `false`）、`limit?`（1–20，默认 10） | 开放注册状态、可用模型、额度与限制说明及 `/free-relays/{id}` 链接 |
| `get_daily_updates` | 看降价与避雷 | `days?`（1–7，默认 3）、`kind?`（`all`/`price_drop`/`identity_mismatch`/`stopped`）、`limit?`（1–30，默认 12） | 近期中转站降价、验出身份不符、停运预警 |

### 推荐组合工作流

1. **买中转站前核查**：`get_provider_detail`（未定站点先 `search_providers` 筛 `operating: "active"`）+ `check_provider_verification` + `get_daily_updates`（`kind: "identity_mismatch"` 或 `"stopped"`）。
2. **挑便宜真站 / 测贵不贵**：`compare_model_prices`（若返 `candidates`，改用精确 `slug`/`model_code` 重查）；已知用户当前站名与模型时，直接调 `check_price_fairness`。
3. **找免费额度与套餐**：先 `list_daily_wool`（需 API 加 `api_only: true`）和 `list_free_relays`（可加 `open_only: true`）；重度编程按月用调 `compare_subscription_plans`。

## 方式二：未安装 MCP 时的回退

- **能发 HTTP 请求**：直接 `POST https://mozhenzhen.com/api/mzz/mcp`（`Content-Type: application/json`，免密钥；遇 HTTP `429` 按 `Retry-After` 等待后再试）：

```json
{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"compare_model_prices","arguments":{"model":"claude-opus-4-6","limit":5}}}
```

- **只能读网页**：直接读取只读 Markdown 伴生页与全站索引：
  - `https://mozhenzhen.com/models/{slug}.md`（模型）、`/providers/{slug}.md`（中转站）、`/plans/{slug}.md`（套餐）、`/wool/{slug}.md`（羊毛）
  - `https://mozhenzhen.com/llms-full.txt`（全站索引）、`https://mozhenzhen.com/llms.txt`（简版目录）

## 回答必须遵守的模真真口径

1. **保留数据上下文**：写清币种、计价单位、样本量与观察/核实时间（`updated_at`/`verified_at`/`confirmed_at`），不编造未返回字段。
2. **区分「暂无验真样本」与「身份不符」**：没测过不能说成假站；验出「身份不符」按公开结论实述，不得臆测成「更像某型号」。
3. **热度不是质量排名**：热度仅代表关注度，不代表质量或推荐，不做「绝对首选」「永不翻车」等承诺。
4. **买号商品不点名店铺横评**：只说明与常见价位的差价，不点名具体店铺做对比。
5. **每条结论必附原始链接**：必须带上 `https://mozhenzhen.com/...` 原始链接供用户核对。
6. **绝不索要凭据**：严禁向用户索要 API Key、Token 或账号密码；需自测中转站请引导至 `https://mozhenzhen.com/verify`。
