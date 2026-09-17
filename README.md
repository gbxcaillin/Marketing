# Marketing

Marketing playbooks, audits and the `marketing-audit` Claude Code skill under `.claude/skills/`.

## Composio MCP (Claude Code)

This repo ships a project-scoped MCP config (`.mcp.json`) that connects
[Composio](https://composio.dev) to Claude Code. It uses an env-var reference
rather than a hardcoded key, so nothing secret is committed.

Set your Composio API key in the environment before starting Claude Code:

```bash
export COMPOSIO_API_KEY=sk_your_real_key   # from your Composio dashboard
```

Then, inside Claude Code, run `/mcp` to approve the `composio` server and
authorize the toolkits (HighLevel, Meta Ads, Google Ads, Google Sheets, etc.)
that the marketing-audit skill's tool hooks rely on.

Prefer a personal setup instead of committing it to the repo? Skip `.mcp.json`
and add it at user scope:

```bash
claude mcp add --scope user --transport http composio \
  https://connect.composio.dev/mcp \
  --header "x-consumer-api-key: sk_your_real_key"
```
