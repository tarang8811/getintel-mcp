# GetIntel MCP Connector

[![smithery badge](https://smithery.ai/badge/tarang8811/getintel)](https://smithery.ai/servers/tarang8811/getintel)

[GetIntel](https://app.getintel.ai) is an AI visibility (GEO) platform — it tracks how often
ChatGPT, Perplexity, Gemini, and Google AI Overviews name your brand in buyer conversations,
finds the gaps, and drafts the fixes. This connector lets your own AI agent (Claude, or any
MCP client) read that data and act on it directly, in conversation.

- **Server:** `https://app.getintel.ai/mcp` (remote, `streamable-http`)
- **Auth:** OAuth 2.1 (Dynamic Client Registration + PKCE) — no pasted API keys
- **Registry:** [`io.github.tarang8811/getintel`](https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.tarang8811/getintel) on the Official MCP Registry
- **Privacy policy:** https://app.getintel.ai/privacy
- **Requires:** an active [GetIntel](https://app.getintel.ai) account

This repository holds the connector's public documentation and tool manifest. The server
implementation lives in GetIntel's main application repo.

## Connecting

1. In your MCP client, add a remote server with URL `https://app.getintel.ai/mcp`.
2. Complete the OAuth flow — you'll be redirected to sign in / authorize with your GetIntel
   account and choose which scopes to grant (`read`, `act`).
3. The client discovers available tools automatically via `tools/list` — a read-only token
   only sees the `read` tools below.

Each brand in your account gets its own token, scoped so an agent connected for one brand can
never see another brand's data — see [`tools.json`](./tools.json) for the full schema.

## Tools

Every call is scoped to the authenticated user/brand. `read` tools never mutate anything.

### Read

| Tool | Description |
|---|---|
| `get_visibility` | AI Visibility score (0-100) — trailing 7-day citation rate, with trend and history. |
| `get_ai_answers` | Buyer questions × engine grid — where the brand is open, absent, mentioned, or recommended. |
| `get_receipts` | The exact AI answer for one buyer question, across engines. |
| `list_activity` | Recent things the agent found or did, most recent first. |
| `list_missions` | Proposed/active batches of related opportunities awaiting approval. |
| `get_mission` | Read-only preview of one mission before approving it. |
| `get_competitors` | Share-of-voice vs named competitors over 30 days. |
| `get_prompt_matrix` | Per-prompt × per-engine state matrix. |
| `get_sources` | Domains AI engines cite in your category — where to earn citations. |
| `get_content_gaps` | Per-question coverage: do you have a page, does AI cite it, what to do. |
| `get_signals` | Reddit/X threads AI cites where you're absent, or competitor complaints. |
| `get_search_console` | Google Search Console performance joined against tracked AI prompts. |
| `get_authority` | Domain rating, referring domains, and link-building opportunities. |
| `get_brand` | Does AI trust/recommend the brand by name — the reputation signal. |
| `get_brand_transcript` | Full untruncated prompt/answer/citation text behind `get_brand`. |
| `get_crawlability` | Can AI crawlers actually reach and read the site (12 pass/warn/fail checks). |
| `get_usage` | This month's MCP read/draft budget and when it resets. |
| `get_prompt_history` | Buyer questions won/lost vs the prior 7-day window. |
| `list_drafts` | Drafts the agent has written (outreach, articles, llms.txt, replies). |
| `get_draft` | Full content of one draft. |

### Act

| Tool | Description |
|---|---|
| `approve_mission` | Approve and execute a mission's whole batch. Not reversible once run. |
| `dismiss_mission` | Dismiss a proposed mission without running it. |
| `replan` | Re-scan current signals and propose fresh missions now. |
| `approve_event` | Approve one individual finding, writing its draft. |
| `dismiss_event` | Dismiss one individual finding. |

`act` tools that send outbound email or publish content still require the account's explicit
send/publish mandate — a connected agent can never grant itself that autonomy.

## Support

Issues or questions: [tarang@sidepanda.com](mailto:tarang@sidepanda.com)
