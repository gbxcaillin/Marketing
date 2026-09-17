# Marketing

Marketing playbooks, audits and the `marketing-audit` Claude Code skill under `.claude/skills/`.

## Composio MCP (Claude Code)

This repo ships a project-scoped MCP config (`.mcp.json`) that connects
[Composio](https://composio.dev) to Claude Code over its OAuth-authenticated
MCP endpoint. There is no API key in the config: Claude Code authenticates you
through Composio's OAuth flow, so nothing secret is committed and each user
signs in as themselves.

Setup:

1. Open this repo in Claude Code and run `/mcp`.
2. Select the `composio` server and choose **Authenticate**. A browser window
   opens Composio's sign-in; approve it.
3. Back in `/mcp`, authorize the toolkits (HighLevel, Meta Ads, Google Ads,
   Google Sheets, SendGrid/Brevo, Firecrawl, etc.) that the marketing-audit
   skill's tool hooks rely on.

Prefer to configure it yourself at user scope instead of using the committed
file? Add the same OAuth endpoint by hand:

```bash
claude mcp add --scope user --transport http composio https://connect.composio.dev/mcp
```

Then run `/mcp` and authenticate as above.

### CLI alternative

Composio also ships a CLI that Claude Code can drive directly via a skill,
instead of the MCP server:

```bash
curl -fsSL https://composio.dev/install | sh   # pin a version if the latest lookup fails, e.g. `... | sh -s -- 0.4.1`
composio login
composio --install-skill claude
```
