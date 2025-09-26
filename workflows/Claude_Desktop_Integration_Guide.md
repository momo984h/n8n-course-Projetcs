# Claude Desktop Integration with n8n

## Installation Guide

Add the following configuration to your Claude Desktop settings to enable n8n integration:

```json
{
  "mcpServers": {
    "n8n": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://elshamyn8n.shop/mcp/emails1994"
      ],
      "env": {}
    }
  }
}
```

After adding the configuration, restart Claude Desktop to activate the MCP connection.