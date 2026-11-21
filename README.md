<p align="center">
  <img src="assets/xainvault-logo-512.png" alt="Xainvault" width="96" height="96">
</p>

<h1 align="center">Xainvault MCP</h1>

<p align="center">
  Connect your AI agents to <a href="https://xainvault.com">Xainvault</a> over MCP.
  <br>
  <a href="https://xainvault.com/docs">Docs</a> ·
  <a href="https://xainvault.com/docs/connect-agents">Connect through MCP</a> ·
  <a href="skills/">Example skills</a>
</p>

## What Xainvault is

Xainvault is a private vault for AI skills. You bring existing skills into one
controlled place, organize them, review them, approve trusted versions, and let
your AI agents install exactly what you approved, nothing else.

Upload a skill. Xainvault scans the files, checks your policy and waits for a
reviewer. Then your team's AI tools get that exact version, delivered over MCP
to ChatGPT, Claude, Cursor, Codex, VS Code, Windsurf and Antigravity. Agents see
granted **Ready** versions by default.

This repository contains public connection guides and example skills only. It
holds no application code. The product documentation lives at
[xainvault.com/docs](https://xainvault.com/docs).

## Quick start

Use this exact address whenever a client asks for an MCP server URL:

```text
https://xainvault.com/mcp
```

Do not add another path or a trailing slash. There are two connection methods:

| Client | How it authenticates |
| --- | --- |
| ChatGPT, Claude (web, Desktop, Cowork, mobile) | Your Xainvault account through OAuth. Do not create or paste an access key. |
| Claude Code, Codex, Cursor, VS Code, Windsurf, Antigravity | An access key created for that machine or environment. |

### OAuth discovery

Hosted connectors (ChatGPT and Claude) discover how to sign in from the
protected resource metadata that Xainvault publishes:

```bash
curl https://xainvault.com/.well-known/oauth-protected-resource
```

The document identifies the resource (`https://xainvault.com/mcp`), the
authorization server to use, the supported scopes (`openid`, `email`,
`profile`), that tokens are sent in a header, and links back to the
[connection docs](https://xainvault.com/docs/connect-agents). This follows the
[MCP authorization specification](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization),
which is based on OAuth 2.1.

You never handle a token yourself. When you add the connector, the client opens
Xainvault, you sign in, review the permissions and choose one vault. During
consent, Xainvault shows the capabilities it is enabling: reading and
installing granted skills, requesting access, submitting your own Drafts for
review, and deciding requests only where your live vault role or reviewer
assignment already allows it. Connector permissions never turn a normal member
into an admin or grant access to a private skill.

### Access keys

Direct clients send an access key in the `X-API-Key` header. In Xainvault, open
**Connections → Access key**, choose your client and create one key for the
machine or environment you are configuring. Xainvault then shows the setup for
that client.

- The full key is shown once. Keep the page open until setup is complete.
- Never commit the key to a repository. The snippets below use the placeholder
  `xv_key_...`; replace it with your own key locally.
- Keys are bound to one vault and scoped to `skills:read` plus Draft-only
  `skills:write`.
- Keys expire after 30, 90 or 180 days, chosen at creation. See
  [Access keys](https://xainvault.com/docs/access-keys) for rotation.

## Connect with your account (OAuth)

### ChatGPT

Custom MCP app availability, who can create one and who can use a published
app depend on your ChatGPT plan and workspace administration. See OpenAI's
[developer mode and MCP apps guide](https://help.openai.com/en/articles/12584461-developer-mode-apps-and-full-mcp-connectors-in-chatgpt-beta).

1. Confirm that your plan supports custom MCP apps and that a workspace admin
   has allowed **Developer mode**.
2. Open **Settings → Apps → Create**. Admins and owners can instead use
   **Workspace settings → Apps → Create**.
3. Name the app **Xainvault**, paste `https://xainvault.com/mcp`, choose OAuth
   authentication and select **Scan Tools**.
4. Sign in to Xainvault, review the permissions and choose one vault. After the
   scan finishes, select **Create**.
5. Start a new chat and enable the Xainvault app from the tools menu. A
   workspace admin must publish the draft before other members can use it.

**Check it:** ask "List my Xainvault skills." Only granted Ready skills from the
selected vault should appear.

### Claude (web, Desktop, Cowork and mobile)

One remote connector covers Claude on the web, Claude Desktop, Cowork and
mobile. Add it once for your Claude account; Desktop is not a second Xainvault
connection. See Claude's
[custom connector guide](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp)
for plan and organization availability.

1. On an individual plan, open **Customize → Connectors**, select
   **+ → Add custom connector**, and enter `https://xainvault.com/mcp`.
2. Name it **Xainvault** and select **Add**.
3. Select **Connect**, sign in to Xainvault and choose one vault.
4. In a new conversation, open **+ → Connectors** and enable Xainvault.

You do not need to edit `claude_desktop_config.json` for this connection.

On Team or Enterprise, an Owner must first open
**Organization settings → Connectors → Add → Custom → Web** and register the
Xainvault URL. Members then open **Customize → Connectors** and select
**Connect** beside Xainvault.

**Check it:** ask "List my Xainvault skills." If the connector is connected but
no tools appear, enable Xainvault for that conversation.

## Connect with an access key

### Claude Code

Copy the generated command from Xainvault. Its shape is:

```bash
claude mcp add --transport http --scope user xainvault \
  https://xainvault.com/mcp \
  --header "X-API-Key: xv_key_..."
```

Open Claude Code and run `/mcp`. Xainvault should appear as connected. See the
[Claude Code MCP documentation](https://docs.claude.com/en/docs/claude-code/mcp)
for commands and scopes.

### Codex

Add the generated block to `~/.codex/config.toml`:

```toml
[mcp_servers.xainvault]
url = "https://xainvault.com/mcp"
http_headers = { "X-API-Key" = "xv_key_..." }
```

Restart the ChatGPT desktop app, Codex CLI or Codex IDE extension after saving;
they share this configuration. Run `/mcp` or `codex mcp list` to verify it. See
the [Codex MCP documentation](https://developers.openai.com/codex/mcp).

### Cursor

Open or create `~/.cursor/mcp.json` for a global, machine-only connection:

```json
{
  "mcpServers": {
    "xainvault": {
      "url": "https://xainvault.com/mcp",
      "headers": { "X-API-Key": "xv_key_..." }
    }
  }
}
```

Open **Cursor Settings → MCP** and confirm Xainvault is enabled. Use
`.cursor/mcp.json` only when a project-scoped connection is intentional, and do
not commit a real key. See the
[Cursor MCP documentation](https://docs.cursor.com/context/model-context-protocol).

### VS Code (GitHub Copilot)

Run **MCP: Open User Configuration** from the Command Palette and paste:

```json
{
  "inputs": [
    {
      "id": "xainvault-key",
      "type": "promptString",
      "description": "Xainvault access key",
      "password": true
    }
  ],
  "servers": {
    "xainvault": {
      "type": "http",
      "url": "https://xainvault.com/mcp",
      "headers": { "X-API-Key": "${input:xainvault-key}" }
    }
  }
}
```

Run **MCP: List Servers**, start Xainvault and paste the one-time key when VS
Code prompts for it. The user configuration avoids placing a secret in a shared
`.vscode/mcp.json` file. See the
[VS Code MCP configuration reference](https://code.visualstudio.com/docs/agents/reference/mcp-configuration).

### Windsurf

Open **Windsurf Settings → Cascade → MCP Servers**, or edit
`~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "xainvault": {
      "serverUrl": "https://xainvault.com/mcp",
      "headers": { "X-API-Key": "xv_key_..." }
    }
  }
}
```

Save the file and refresh the MCP server list. See the
[Windsurf MCP documentation](https://docs.windsurf.com/windsurf/cascade/mcp).

### Antigravity

Open **MCP Servers → Manage MCP Servers → View raw config**, or edit
`~/.gemini/config/mcp_config.json`:

```json
{
  "mcpServers": {
    "xainvault": {
      "serverUrl": "https://xainvault.com/mcp",
      "headers": { "X-API-Key": "xv_key_..." }
    }
  }
}
```

Save the file, then open **Settings → Customizations** and refresh MCP servers.
See Google's
[Antigravity getting-started codelab](https://codelabs.developers.google.com/getting-started-google-antigravity).

## What agents can do

Through MCP, connected agents can discover the skills and collections they are
allowed to see, read each skill's `SKILL.md` and file listing, and install
approved packages. Your role, group memberships and per-skill grants decide what
each agent can list and pull.

Agents install full packages (`SKILL.md` plus any bundled references, scripts
and assets) through a signed ZIP download that stays valid for 15 minutes and
comes with a SHA-256 checksum to verify. Every connection can also save an
AI-authored package to Xainvault as a Draft, for example: "Create a skill for
reviewing incident reports and save it to my Xainvault." Drafts do not become
Ready without the normal scan and review.

Details: [Connect through MCP](https://xainvault.com/docs/connect-agents).

## Troubleshooting

See [docs/troubleshooting.md](docs/troubleshooting.md).

## Example skills

[`skills/`](skills/) contains small example skills in the `SKILL.md` format
Xainvault imports, and explains how a skill becomes a Ready version.

## More documentation

- [How Xainvault works](https://xainvault.com/docs)
- [Quickstart](https://xainvault.com/docs/quickstart)
- [Connect through MCP](https://xainvault.com/docs/connect-agents)
- [Access keys](https://xainvault.com/docs/access-keys)
- [Skills & versions](https://xainvault.com/docs/skills-and-versions)
- [Skill security](https://xainvault.com/docs/skill-security)

## License

[MIT](LICENSE)
