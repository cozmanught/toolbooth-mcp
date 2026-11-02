# Installing Toolbooth (for AI coding assistants)

Toolbooth is a hosted remote MCP server. There is nothing to clone, build or run.

1. Ask the user for their Toolbooth Agent Kiosk API key. It starts with `tb_live_`. If they don't
   have one, tell them they can get one at https://toolbooth.io/kiosk (it costs $2 and includes
   $1.00 of credit). Don't create one for them, and don't print the key back.
2. Add this server to the MCP settings file (for Cline: `cline_mcp_settings.json`, opened with
   "Configure MCP Servers"), with the key in place of `tb_live_...`:

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

3. Check the connection by calling `compute_math` with `{"expression": "2+2"}` (it costs $0.0005
   of credit).

Notes:

- Every tool call is paid. Each tool's price is in its description ($0.0005 to $0.10 per call).
  Failed calls are refunded.
- Leave `autoApprove` empty so the user approves each paid call, unless they ask otherwise.
- If a call returns "Payment required", the key is missing, wrong or out of credit. Stop and tell
  the user; credit can be added on the agent's passport page.
- Without a key the server still connects and lists tools, but every call returns x402 payment
  requirements instead of a result.
