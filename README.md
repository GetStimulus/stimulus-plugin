# Stimulus Claude Code Plugin

Procurement intelligence plugin for Claude Code. Provides 7 MCP tools, 10 procurement workflow skills, and a specialized procurement analyst agent.

## Prerequisites

1. **Stimulus dev servers running:**
   ```bash
   bun run dev   # starts API (port 8000) + Web/BFF (port 3000)
   ```
2. **Clerk OAuth Application** enabled in Clerk Dashboard (for OAuth flow)
3. **Dependencies installed** (`bun install` in monorepo root)

## Authentication

The plugin uses **OAuth 2.1 with PKCE** via Clerk. When you first use a tool, Claude Code opens a browser for Clerk/Google login. No API keys or env vars needed on your machine.

### How OAuth Works

1. Plugin connects to the MCP HTTP endpoint (`/api/mcp`)
2. Server returns `401` with `WWW-Authenticate` pointing to `/.well-known/oauth-protected-resource/mcp`
3. Claude Code discovers Clerk's authorization server via `/.well-known/oauth-authorization-server`
4. You authenticate in the browser (Google login via Clerk)
5. Claude Code receives an OAuth token and sends it with each MCP request
6. Server validates the token, resolves your tenant from JWT claims
7. Tools call the BFF with your Bearer token, which forwards to FastAPI

### Custom MCP URL

By default the plugin connects to `http://localhost:3000/api/mcp`. Override with:

```bash
STIMULUS_MCP_URL=https://console.getstimulus.ai/api/mcp claude --plugin-dir ./stimulus-plugin
```

### Production

For deployed environments, set `STIMULUS_MCP_URL`:

```bash
STIMULUS_MCP_URL=https://console.getstimulus.ai/api/mcp claude --plugin-dir ./stimulus-plugin
```

### Falling Back to Stdio (M2M Tokens)

If you need stdio transport instead of OAuth (e.g., for CI or environments without a browser), use the MCP CLI directly:

```bash
claude mcp add stimulus \
  -e STIMULUS_TENANT_ID=org_2tXuPzB4PVasAeTPTujeppDYl8b \
  -- npx tsx apps/web/src/mcp/server/stdio.ts
```

This requires `CLERK_SECRET_KEY` and `CLERK_MACHINE_SECRET_KEY` in your environment.

## Installation

From the monorepo root:

```bash
claude --plugin-dir ./stimulus-plugin
```

Or add to your Claude Code settings for persistent use.

## Available Skills (Slash Commands)

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

The plugin exposes 7 MCP tools:

| Tool | Description |
|------|-------------|
| `supplier_search` | AI-powered semantic supplier search with filters |
| `portfolio_health` | Portfolio-wide relationship health overview |
| `load_relationship` | Detailed supplier relationship data |
| `log_interaction` | Record supplier interactions |
| `supplier_performance` | Supplier journey and performance metrics |
| `spend_analysis` | Spend analytics (metrics, trends, diversity, industries, top suppliers) |
| `project_info` | Procurement project and RFP management |

## Procurement Analyst Agent

The plugin includes a specialized `procurement-analyst` agent that auto-activates for procurement questions. It has access to all 7 MCP tools and understands:

- Health score interpretation (Healthy/Watch/At Risk/Critical thresholds)
- Diversity compliance targets (MBE, WBE, DBE, SDVOB)
- Contract renewal decision framework
- Risk assessment methodology
- RFP evaluation scoring dimensions

## Troubleshooting

### OAuth login doesn't open
Ensure the Next.js dev server is running at the URL in `.mcp.json` (default `http://localhost:3000`).

### Tools return 401/403
1. Check that the BFF is running (`bun run web:dev`)
2. Try reconnecting — your OAuth token may have expired
3. Verify Clerk OAuth Application is enabled in the Clerk Dashboard

### "Connection refused" errors
The plugin connects to `http://localhost:3000/api/mcp` by default. Ensure both dev servers are running (`bun run dev`).

### Checking MCP server logs
In Claude Code, MCP server logs appear in the MCP panel. The HTTP transport logs to the Next.js server console.

## Architecture

```
Claude Code
  |
  |- stimulus-plugin/
  |   |- .claude-plugin/plugin.json   (manifest)
  |   |- .mcp.json                    (HTTP transport + OAuth)
  |   |- skills/                      (10 workflow skills)
  |   |- agents/                      (procurement analyst agent)
  |   '- settings.json                (agent auto-invocation)
  |
  '- MCP HTTP Transport (OAuth 2.1 PKCE)
      |
      '- POST /api/mcp
          |
          |- Clerk OAuth token validation
          |- Tenant resolved from JWT claims
          |
          '- BFF Proxy (Next.js :3000)
              |
              '- FastAPI Backend (:8000)
                  |
                  '- PostgreSQL (multi-tenant)
```

### OAuth Metadata Endpoints

| Endpoint | RFC | Purpose |
|----------|-----|---------|
| `/.well-known/oauth-protected-resource/mcp` | RFC 9728 | Tells clients which auth server to use |
| `/.well-known/oauth-authorization-server` | RFC 8414 | Clerk's OAuth endpoints (authorize, token, JWKS) |
