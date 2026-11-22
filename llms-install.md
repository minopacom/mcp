# Installing the Minopa MCP server

Minopa is a hosted remote MCP server. There is nothing to download, build or run.

1. Add a remote MCP server named `minopa` with the Streamable HTTP URL `https://mcp.minopa.com/mcp`.
2. Authentication is OAuth 2.1 with dynamic client registration. The client opens the Minopa sign-in page; the user signs in and picks a workspace. No API key is needed.
3. If the client can't do OAuth, the user can create an API key in Minopa under Settings → API keys and the client sends it as the header `Authorization: Bearer <key>`. Ask the user for the key; never invent one.

Example configuration for clients that use an `mcpServers` object:

```json
{
  "mcpServers": {
    "minopa": {
      "type": "streamable-http",
      "url": "https://mcp.minopa.com/mcp"
    }
  }
}
```

After connecting, call `list_social_accounts` to confirm it works. The user needs a Minopa account (free plan available at https://minopa.com) with at least one social account connected.
