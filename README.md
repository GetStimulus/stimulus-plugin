# Stimulus Plugin for Claude Code

Procurement intelligence plugin for [Claude Code](https://claude.ai/code). Provides 7 MCP tools, 10 procurement workflow skills, and a specialized procurement analyst agent — all connected to your Stimulus account via OAuth.

## Quick Start

### Install from Marketplace

```bash
/plugin marketplace add GetStimulus/stimulus-plugin
/plugin install stimulus@stimulus-plugin
```

### Or use locally (development)

```bash
claude --plugin-dir ./stimulus-plugin
```

## Authentication

The plugin uses **OAuth 2.1 with PKCE** via Clerk. When you first use a tool:

1. Claude Code connects to the Stimulus MCP endpoint
2. You authenticate in the browser (sign in via Clerk)
3. Your OAuth token is used for all subsequent requests
4. Your organization and tenant are resolved automatically from your account

No API keys or environment variables needed.

### Custom MCP URL

By default the plugin connects to `https://console.getstimulus.ai/api/mcp`. For local development, override with:

```bash
STIMULUS_MCP_URL=http://localhost:3000/api/mcp claude --plugin-dir ./stimulus-plugin
```

### Stdio Transport (CI / Headless)

For environments without a browser (CI pipelines, remote servers), use stdio transport with M2M tokens:

```bash
claude mcp add stimulus \
  -e STIMULUS_TENANT_ID=<your-clerk-org-id> \
  -- npx tsx apps/web/src/mcp/server/stdio.ts
```

Requires `CLERK_SECRET_KEY` and `CLERK_MACHINE_SECRET_KEY` in your environment. Contact your Stimulus admin for credentials.

## Skills

| Command | Description |
|---------|-------------|
| `/stimulus:morning-briefing` | Daily executive procurement briefing |
| `/stimulus:supplier-discovery` | AI-powered supplier search and qualification |
| `/stimulus:interaction-logging` | Log supplier interactions with follow-up |
| `/stimulus:weekly-intelligence` | Weekly portfolio intelligence report |
| `/stimulus:diversity-compliance` | Diversity spend audit against targets |
| `/stimulus:contract-renewal` | Renew/renegotiate/re-bid decision |
| `/stimulus:rfp-lifecycle` | Full RFP lifecycle management |
| `/stimulus:vendor-onboarding` | New supplier onboarding checklist |
| `/stimulus:risk-assessment` | Supplier risk scoring and mitigation |
| `/stimulus:spend-reconciliation` | Budget variance and concentration analysis |

## MCP Tools

| Tool | Description |
|------|-------------|
| `supplier_search` | Semantic supplier search with location, certification, and industry filters |
| `portfolio_health` | Portfolio-wide relationship health overview |
| `load_relationship` | Detailed supplier relationship data |
| `log_interaction` | Record supplier interactions |
| `supplier_performance` | Supplier journey and performance metrics |
| `spend_analysis` | Spend analytics — metrics, trends, diversity breakdown, top suppliers |
| `project_info` | Procurement project and RFP management |

## Procurement Analyst Agent

The plugin includes a `procurement-analyst` agent that auto-activates for procurement questions. It understands supplier health scoring, diversity compliance, contract renewal decisions, risk assessment, and RFP evaluation.

## Troubleshooting

### OAuth login doesn't open
Ensure the Stimulus server is running at the URL configured in `.mcp.json`.

### Tools return 401/403
- Your OAuth token may have expired — reconnect by running a tool again
- Verify your Clerk account has access to the target organization

### "Connection refused"
The default endpoint is `http://localhost:3000/api/mcp`. Make sure the dev server is running (`bun run dev` in the Stimulus monorepo).

## Updating

```bash
/plugin update stimulus@stimulus-plugin
```

Claude Code checks for updates on session start. Bump the version in `plugin.json` to publish updates.

## License

Proprietary. For use by authorized Stimulus platform users only.
