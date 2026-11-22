# Minopa MCP server

Draft, schedule and publish social media posts from Claude, ChatGPT, Cursor, VS Code or any MCP client. Minopa connects to Instagram, TikTok, Threads, Facebook Pages, X, LinkedIn, Pinterest and YouTube, so you can ask your assistant to write a post, schedule it for next week, check why a post was rejected, or see which posts did best.

This repository holds the connection details for the hosted Minopa MCP server and a Claude plugin with a skill that teaches Claude the posting flow. The server itself runs at Minopa; there is nothing to install or run locally.

- **Endpoint:** `https://mcp.minopa.com/mcp` (Streamable HTTP)
- **Sign-in:** OAuth 2.1 with dynamic client registration and PKCE. You sign in with your Minopa account and pick one workspace; the assistant only sees that workspace.
- **Account:** a Minopa account ([sign up free](https://minopa.com/en/signup?utm_source=mcp-github&utm_medium=readme), no card; see [pricing](https://minopa.com/en/pricing?utm_source=mcp-github&utm_medium=readme)) with at least one social account connected in the Minopa app.
- **Docs:** [minopa.com/en/mcp](https://minopa.com/en/mcp?utm_source=mcp-github&utm_medium=readme)

## Connect

### Claude (claude.ai, Desktop, mobile)

Minopa is in the Claude connector directory: open **Settings → Connectors**, search for **Minopa** and click **Connect**. You can also add it as a custom connector with the endpoint above.

### Claude Code

Install the plugin from this repository, which adds the MCP server and the posting skill:

```
/plugin marketplace add minopacom/mcp
/plugin install minopa@minopa
```

Or add only the server:

```
claude mcp add --transport http minopa https://mcp.minopa.com/mcp
```

Then run `/mcp`, select **minopa** and choose **Authenticate**.

### Cursor

Use the **Add to Cursor** button on [minopa.com/en/mcp](https://minopa.com/en/mcp?utm_source=mcp-github&utm_medium=readme#setup), or add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "minopa": { "url": "https://mcp.minopa.com/mcp" }
  }
}
```

### VS Code

Use the **Install in VS Code** button on [minopa.com/en/mcp](https://minopa.com/en/mcp?utm_source=mcp-github&utm_medium=readme#setup), or add this to `.vscode/mcp.json`:

```json
{
  "servers": {
    "minopa": { "type": "http", "url": "https://mcp.minopa.com/mcp" }
  }
}
```

### ChatGPT

In ChatGPT, open **Settings → Apps & Connectors → Advanced**, turn on developer mode, then create a connector with the endpoint above.

### n8n, scripts and other clients

Clients without OAuth can send an API key instead: create one in Minopa under **Settings → API keys** and send it as `Authorization: Bearer <key>`. The REST API is documented at [developers.minopa.com](https://developers.minopa.com).

## What you can ask

- "Which social accounts are connected to Minopa?"
- "Write a LinkedIn post about our new feature and schedule it for Tuesday at 9:00."
- "Upload this image and save a draft Instagram post with it."
- "What's scheduled for next week, and why was my last TikTok post rejected?"
- "Which of last month's posts got the most engagement?"

## Tools

The server exposes tools for the workspace (usage and AI credits), connected accounts, posts (list, create, update, publish, delete), the media library (upload from a URL, platform media specs, conversion, folders), Pinterest boards, YouTube playlists and categories, and post analytics on plans that include it. Every tool is annotated: read-only tools are marked as such, and publishing, updating and deleting are marked destructive, so clients ask before running them.

## What this plugin does and sends

- The plugin contains no code. It adds one remote MCP server (`https://mcp.minopa.com/mcp`) and one skill (`skills/social-posting/SKILL.md`), which is plain instructions for Claude.
- When you use a tool, your request goes to Minopa's server, which acts on your Minopa workspace and, when you publish, sends the post to the social network through that network's official API, using the account you connected in Minopa.
- Minopa's [privacy policy](https://minopa.com/en/privacy) and [terms](https://minopa.com/en/terms) apply. Questions: [info@minopa.com](mailto:info@minopa.com) or [minopa.com/en/contact](https://minopa.com/en/contact).

## Registry

Published in the official MCP Registry as `com.minopa/mcp` ([server.json](server.json)).

## License

MIT, see [LICENSE](LICENSE).
