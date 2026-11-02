# Toolbooth

Pay-per-call MCP tools for AI agents. One remote MCP endpoint, 60 tools: Solana and DEX data,
perps and options data, prediction markets, DeFi yields and risk, cross-chain gas and bridges,
web search and scraping, and dev security checks. No accounts or subscriptions: each call is paid
in USDC on Solana via [x402](https://x402.org), or from prepaid Agent Kiosk credit.

- MCP endpoint: `https://toolbooth.io/mcp` (Streamable HTTP)
- Site: https://toolbooth.io · Docs: https://toolbooth.io/docs · For agents: https://toolbooth.io/for-agents
- llms.txt: https://toolbooth.io/llms.txt · Server card: https://toolbooth.io/mcp/server-card
- Official MCP Registry: `io.toolbooth/toolbooth`
- X: [@toolbooth402](https://x.com/toolbooth402)

This repository holds the connection files for directories and MCP clients, and a Claude Code
plugin. The server is hosted by Toolbooth; there is nothing to build or run.

- `.claude-plugin/marketplace.json` and `plugins/toolbooth/`: the Claude Code plugin (MCP server
  config and the `toolbooth` skill in `plugins/toolbooth/skills/toolbooth/SKILL.md`)
- `.mcp.json` and `.cursor-plugin/plugin.json`: Cursor
- `llms-install.md`: Cline and other AI installers
- `lhm.plugin.json`: LobeHub

## Connect

Most MCP clients can't sign x402 payments themselves, so they connect with an Agent Kiosk API key
(`tb_live_…`) and spend prepaid credit. Get one at https://toolbooth.io/kiosk ($2, which includes
$1.00 of credit). For the configs below, put it in the `TOOLBOOTH_API_KEY` environment variable.
Never paste the key into a file you commit.

Claude Code, as a plugin (the MCP server plus a skill on which tool to use and what it costs):

```
/plugin marketplace add OWNER/REPO
/plugin install toolbooth@toolbooth
```

Claude Code asks for the API key when the plugin is enabled and keeps it in secure storage, not in
a settings file. To set or change it later: `/plugin configure toolbooth@toolbooth`. Then run
`/reload-plugins` (or start a new session) and check `/mcp`: the server shows as
`plugin:toolbooth:toolbooth`. The skill is `/toolbooth:toolbooth`, and Claude also loads it on its
own when a question fits. From a shell, the same steps are
`claude plugin marketplace add OWNER/REPO` and `claude plugin install toolbooth@toolbooth`.

Claude Code, MCP server only:

```bash
export TOOLBOOTH_API_KEY=tb_live_...
claude mcp add --transport http --scope user toolbooth https://toolbooth.io/mcp \
  --header "Authorization: Bearer $TOOLBOOTH_API_KEY"
```

Cursor (`~/.cursor/mcp.json` or `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "toolbooth": {
      "url": "https://toolbooth.io/mcp",
      "headers": { "Authorization": "Bearer ${env:TOOLBOOTH_API_KEY}" }
    }
  }
}
```

Cline (`cline_mcp_settings.json`, via "Configure MCP Servers"):

```json
{
  "mcpServers": {
    "toolbooth": {
      "type": "streamableHttp",
      "url": "https://toolbooth.io/mcp",
      "headers": { "Authorization": "Bearer tb_live_..." },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

VS Code, Codex CLI, ElizaOS, OpenClaw, LangChain, OpenAI Agents SDK, Vercel AI SDK and CrewAI:
see https://toolbooth.io/start.

Without a key, any MCP client can still connect, list the tools and call them: an unpaid call
returns the x402 payment requirements instead of a result. An x402 client (for example
`@x402/mcp`) signs the USDC payment and retries. Details: https://toolbooth.io/for-agents.

## Pricing

Each tool has a fixed price per call, shown in its description: $0.0005 to $0.10, most $0.01 or
less. Failed calls are not charged (credit is refunded). Prices: https://toolbooth.io/docs.

## Limits

60 requests per minute per MCP session (HTTP 429 with `Retry-After`).

Nothing Toolbooth returns is financial advice.
