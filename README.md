# Rams: design review for UI code, inside your agent

[Rams](https://www.rams.ai) reviews UI code the way a design-literate senior
engineer would: severity-rated findings with file and line, a 0–100 score,
and a concrete fix for each issue. It runs as a hosted
[MCP](https://modelcontextprotocol.io) server, so Claude Code, Cursor, Codex,
and any MCP-capable agent can call it while they work, before the code is
ever committed.

This repository is the connection guide. The engine is hosted. There is
nothing to install or run locally.

## Tools

| tool | what it does | cost |
|---|---|---|
| `quick_review` | Top issues on the files you just changed, ~10 seconds | 1/5 review credit |
| `review_files` | Full scored review: 0–100, every issue with a fix, git-apply-able patches | 1 credit |
| `verify_fixes` | Confirms applied fixes actually landed | free |
| `usage` | Credits used and remaining | free |

The engine reviews React, Vue, Svelte, CSS, and SwiftUI against a rule set
spanning accessibility, hierarchy, color, type, spacing, motion, and
native-platform conventions. The live rule count is at
[rams.ai/rules](https://www.rams.ai/rules).

## Setup

Get a key at [rams.ai](https://www.rams.ai) (free tier included, no card).

**Claude Code**

```bash
claude mcp add --transport http rams https://worker.rams.ai/mcp --header "Authorization: Bearer rams_YOUR_KEY"
```

**Cursor** · `.cursor/mcp.json`

```json
{
  "mcpServers": {
    "rams": {
      "url": "https://worker.rams.ai/mcp",
      "headers": { "Authorization": "Bearer rams_YOUR_KEY" }
    }
  }
}
```

**Codex** · `~/.codex/config.toml`

```toml
[mcp_servers.rams]
url = "https://worker.rams.ai/mcp"
http_headers = { "Authorization" = "Bearer rams_YOUR_KEY" }
```

Any other MCP-capable agent: Streamable HTTP at `https://worker.rams.ai/mcp`
with the key as a Bearer header (`?apiKey=` also works for gateways that
can't set headers).

Then ask your agent:

```
rams review my changes
```

## How it behaves

- Quick checks are built to run constantly while editing: after writing a
  component, after a refactor, before moving on.
- Full reviews are the commit-time artifact: the score you keep, the patches
  you apply.
- Criticals cap the score: one caps it at 59, two at 49, three or more
  at 39. A 90+ means something.
- Your code is reviewed and returned, never stored.

## Elsewhere

- Web: [rams.ai](https://www.rams.ai) · Rules: [rams.ai/rules](https://www.rams.ai/rules) · Pricing: [rams.ai/pricing](https://www.rams.ai/pricing)
- Official MCP registry: `ai.rams/rams` · [Glama](https://glama.ai/mcp/connectors/ai.rams.worker/rams) · [Smithery](https://smithery.ai/server/@eli-rousso/rams) · [mcp.so](https://mcp.so/servers/rams)
- GitHub App (reviews every PR automatically): [rams.ai](https://www.rams.ai)

## License

This connection guide is MIT licensed. The hosted engine is a commercial service by [HSLA0001 Inc.](https://www.rams.ai)
