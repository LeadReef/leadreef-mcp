# Connect LeadReef to your AI app

Server URL for every app:

```
https://api.leadreef.ai/mcp
```

You sign in on leadreef.ai and click Allow. No API key, nothing to install. Ready-made config files are in [`configs/`](../configs).

## Claude

Claude.ai and the Claude desktop app, on every plan. Free includes one custom connector.

1. **Open Customize → Connectors.** In Claude, open Customize, then Connectors.
2. **Click + Add → “Add custom connector”.** Name it LeadReef and paste the server URL.
3. **Sign in to LeadReef and click Allow.** A LeadReef window shows exactly what Claude will be able to do.
4. **Turn LeadReef on in a chat.** Click + at the lower left of the message box → Connectors, then ask for leads.

Server URL:

```
https://api.leadreef.ai/mcp
```

## ChatGPT

ChatGPT on the web. Custom MCP servers are not available in the mobile apps.

1. **Open ChatGPT Plugins.** Go to chatgpt.com/plugins and click +, then “Add custom MCP server”.
2. **Paste the server URL.** Name it LeadReef, paste the URL under Connection, choose OAuth, then “Create as a plugin”.
3. **Sign in to LeadReef and click Allow.** You come back to ChatGPT once it is connected.
4. **Pick LeadReef in a chat.** Type @ in the message box, choose LeadReef, then ask for leads.

Server URL:

```
https://api.leadreef.ai/mcp
```

## Mistral Vibe

Mistral Vibe (formerly Le Chat), in Work.

1. **Open Work → Connectors.** In Mistral Vibe, open the Connectors page.
2. **+ Add Connector → Custom MCP Connector.** Name it leadreef, paste the server URL, click Connect.
3. **Sign in to LeadReef and click Allow.** Vibe detects the sign-in method on its own.
4. **Ask for leads.** Use the connector in a Work chat.

Server URL:

```
https://api.leadreef.ai/mcp
```

## Claude Code

Anthropic's coding agent in your terminal.

1. **Add the server.** Run the command below in your terminal.
2. **Open the MCP menu.** Start Claude Code and type /mcp.
3. **Authenticate.** Pick leadreef → Authenticate. Your browser opens LeadReef.
4. **Approve and ask.** Click Allow, go back to the terminal and ask for leads.

Terminal:

```
claude mcp add --transport http leadreef https://api.leadreef.ai/mcp
```

## Claude Code plugin

Installs the server plus the find-leads skill from this repository.

```
/plugin marketplace add LeadReef/leadreef-mcp
/plugin install leadreef@leadreef
```

Then type /mcp, pick leadreef → Authenticate.

## Cursor

Cursor agent, in any project.

1. **Open Customize → MCP servers.** Add a custom MCP server. It opens mcp.json.
2. **Paste the block below.** Save the file. leadreef appears in the server list.
3. **Sign in when asked.** Cursor opens LeadReef in your browser.
4. **Approve and ask.** Click Allow, then ask the agent for leads.

~/.cursor/mcp.json:

```
{
  "mcpServers": {
    "leadreef": {
      "url": "https://api.leadreef.ai/mcp"
    }
  }
}
```

## VS Code

GitHub Copilot agent mode in VS Code.

1. **Add the server.** Command Palette → “MCP: Add Server” → HTTP, or paste the block below.
2. **Start it.** Save mcp.json. VS Code starts the server when the agent needs it.
3. **Sign in when asked.** VS Code opens LeadReef in your browser. Click Allow.
4. **Ask the agent.** Open Copilot Chat and ask for leads.

.vscode/mcp.json:

```
{
  "servers": {
    "leadreef": {
      "type": "http",
      "url": "https://api.leadreef.ai/mcp"
    }
  }
}
```

## Devin Desktop

Devin Desktop (formerly Windsurf), Devin Local agent.

1. **Add the server.** Run the commands below, or Customize → MCPs → open the config and paste the block.
2. **Authenticate.** leadreef shows “Needs authentication”. Click Authenticate.
3. **Approve in the browser.** Your browser opens LeadReef. Click Allow.
4. **Ask Devin.** LeadReef tools show up in the MCP list.

Terminal:

```
devin mcp add leadreef https://api.leadreef.ai/mcp
devin mcp login leadreef
```

~/.config/devin/mcp_config.json:

```
{
  "mcpServers": {
    "leadreef": {
      "url": "https://api.leadreef.ai/mcp"
    }
  }
}
```

## Gemini CLI

Fastest: install this repository as a Gemini CLI extension, then run `/mcp auth leadreef` inside gemini.

```
gemini extensions install https://github.com/LeadReef/leadreef-mcp
```

Or add the server by hand:

Google's open-source agent in your terminal.

1. **Add the server.** Paste the block into your settings file.
2. **Authenticate.** Start gemini and run the command below.
3. **Approve in the browser.** Click Allow on the LeadReef page.
4. **Ask for leads.** Type /mcp to check the tools are listed.

~/.gemini/settings.json:

```
{
  "mcpServers": {
    "leadreef": {
      "httpUrl": "https://api.leadreef.ai/mcp"
    }
  }
}
```

Inside Gemini CLI:

```
/mcp auth leadreef
```

## Codex

OpenAI Codex CLI and IDE extension.

1. **Add the server.** Paste the block into your Codex config.
2. **Log in.** Run the command below. Your browser opens LeadReef.
3. **Approve.** Click Allow.
4. **Ask for leads.** Start codex and ask.

~/.codex/config.toml:

```
[mcp_servers.leadreef]
url = "https://api.leadreef.ai/mcp"
```

Terminal:

```
codex mcp login leadreef
```

## Any other MCP client

Any client that supports remote MCP servers over Streamable HTTP with OAuth sign-in works. Add `https://api.leadreef.ai/mcp` as a remote (HTTP) server and approve LeadReef when your browser opens.

Source: [leadreef.ai/mcp](https://leadreef.ai/mcp#setup)
