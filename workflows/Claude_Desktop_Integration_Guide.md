# Claude Desktop Integration with n8n

## Overview

This guide provides complete setup instructions for integrating your n8n workflows with Claude Desktop using Model Context Protocol (MCP). This advanced integration enables Claude to directly interact with and control your n8n automation workflows through natural language commands.

## Prerequisites

- n8n instance running and accessible via a production URL
- Claude Desktop installed on your system
- Basic understanding of Model Context Protocol (MCP)
- Production n8n endpoint configured for MCP communication

## Installation Steps

### 1. Configure MCP Server in Claude Desktop

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

### 2. Configuration Details

- **MCP Server Name**: `n8n` - This identifies the server in Claude Desktop
- **Production URL**: `https://elshamyn8n.shop/mcp/emails1994` - Your n8n MCP endpoint
- **Command**: Uses `npx` to run the `mcp-remote` package
- **Environment**: No additional environment variables required

### 3. Restart Claude Desktop

After adding the configuration, restart Claude Desktop to activate the MCP connection.

### 4. Verify Integration

Once configured, Claude Desktop will be able to:
- Access your n8n workflows
- Trigger workflow executions
- Retrieve workflow results
- Monitor automation status
- Create and modify workflows through natural language

## Usage Examples

With the integration active, you can use natural language commands in Claude Desktop like:

- **"Run my customer onboarding workflow"**
- **"Check the status of recent email automations"**
- **"Create a new workflow for social media posting"**
- **"Show me all active workflows"**
- **"Trigger the data backup workflow"**
- **"Update the lead generation automation"**

## Features

### Core Capabilities
- **Workflow Management**: Start, stop, and monitor workflows
- **Data Access**: Retrieve workflow execution data and logs
- **Configuration**: Modify workflow settings and parameters
- **Real-time Status**: Get live updates on automation performance

### Advanced Features
- **Natural Language Interface**: Interact with n8n using conversational commands
- **Intelligent Automation**: Claude can suggest workflow optimizations
- **Error Handling**: Automatic troubleshooting and resolution suggestions
- **Multi-workflow Coordination**: Manage complex automation sequences

## Troubleshooting

### Connection Issues
- Verify your n8n URL is accessible from external networks: `https://elshamyn8n.shop/mcp/emails1994`
- Check firewall settings and port configurations
- Ensure proper SSL certificates are configured
- Test the MCP endpoint accessibility

### Authentication Problems
- Verify API credentials in your n8n instance
- Check webhook authentication settings
- Confirm MCP server permissions
- Validate the production URL format

### Claude Desktop Issues
- Restart Claude Desktop after configuration changes
- Check the configuration syntax in the MCP settings
- Verify that the `mcp-remote` package is accessible via `npx`
- Review Claude Desktop logs for connection errors

## Security Considerations

- **Endpoint Security**: Ensure your n8n MCP endpoint is properly secured
- **Access Control**: Implement appropriate authentication mechanisms
- **Data Privacy**: Review what data is accessible through the MCP interface
- **Network Security**: Use HTTPS and secure network configurations

## Advanced Configuration

### Custom Environment Variables
If needed, you can add environment variables to the configuration:

```json
{
  "mcpServers": {
    "n8n": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://elshamyn8n.shop/mcp/emails1994"
      ],
      "env": {
        "API_KEY": "your-api-key",
        "DEBUG": "true"
      }
    }
  }
}
```

### Multiple Endpoints
You can configure multiple n8n instances:

```json
{
  "mcpServers": {
    "n8n-production": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://elshamyn8n.shop/mcp/emails1994"
      ],
      "env": {}
    },
    "n8n-development": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://dev.elshamyn8n.shop/mcp/emails1994"
      ],
      "env": {}
    }
  }
}
```

## Support

For additional help and advanced configurations:
- **YouTube Tutorial**: Coming Soon
- **GitHub Issues**: Report bugs or request features
- **Community Support**: Join our Discord for real-time help
- **Documentation**: [n8n MCP Documentation](https://docs.n8n.io/mcp)

## Related Resources

- [n8n Official Documentation](https://docs.n8n.io/)
- [Claude Desktop MCP Guide](https://docs.anthropic.com/claude/docs/mcp)
- [Model Context Protocol Specification](https://spec.modelcontextprotocol.io/)
- [n8n Community Forum](https://community.n8n.io/)

---

**Happy Automating with Claude Desktop! 🤖✨**
