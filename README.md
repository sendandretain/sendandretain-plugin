# Send & Retain plugin

Connects an AI coding agent to Send & Retain over MCP. Ships in **two
formats from one tree** — the vendor-neutral
[Agent Plugin](https://agent-plugins.org) spec and Claude Code's — sharing a
single `skills/` directory.

## Install

**Claude Code**

```bash
claude --plugin-url https://sendandretain.com/api/mcp/plugin.zip
```

**Claude Code / any MCP client, without the plugin**

```bash
claude mcp add --transport http sendandretain https://sendandretain.com/api/mcp
```

**Codex** — in `~/.codex/config.toml`:

```toml
[mcp_servers.sendandretain]
type = "http"
url = "https://sendandretain.com/api/mcp"
```

**Cursor / VS Code / Copilot / ChatGPT / Kiro** — point the client at this
repo's `plugin.json`, or add the MCP server URL directly.

**claude.ai** — Settings → Connectors → Add custom connector, paste
`https://sendandretain.com/api/mcp`.

**A client that only speaks stdio** — bridge it, no extra package from us:

```bash
npx mcp-remote https://sendandretain.com/api/mcp
```

Every route authorizes the same way: OAuth 2.1 in a browser. There is no token
to set, and any instruction telling you to export one is out of date.

## What is in here

| Path | |
| --- | --- |
| `plugin.json` / `mcp.json` | Agent Plugin manifest (agent-plugins.org 1.0.0) |
| `.claude-plugin/` | Claude Code manifest + marketplace entry |
| `.mcp.json` | Claude Code MCP config |
| `skills/` | Reference skill: authorization model, tool catalogue, safety model |
| `commands/` | Claude Code slash commands |

The operating playbooks that run server-side are not in this bundle — they are
product IP, and this repo is public.
