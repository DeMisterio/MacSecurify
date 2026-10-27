# Connect an AI agent

MacSecurify's local MCP helper lets a cooperating agent learn which apps and websites are protected, understand a denial, and request a supported capability. It does not expose credentials, biometrics, password recovery, settings edits, or a general unlock command.

## What it helps with

MCP means **Model Context Protocol**: the connection an AI client uses to ask MacSecurify about protection. A cooperating agent can check a website before trying to open it, explain why an app is blocked, and ask for a supported exception instead of repeatedly attempting a denied action.

This connection is optional. Connecting a client does not unlock apps or enable exceptions. Keep MacSecurify installed in Applications and running. Find the connection guide at the bottom of **Security → Agent MCP**.

## Set up from Terminal

Choose the command for your client. Run it yourself, then restart that client. The bundled helper needs no Node.js, Python, provider API key, or additional server package.

### Claude Desktop

Quit Claude Desktop before running setup so it cannot overwrite the configuration while setup is saving it.

```sh
'/Applications/MacSecurify.app/Contents/Helpers/MacSecurifyMCP' --install claude-desktop
```

This explicit setup command merges the `macsecurify` entry into `~/Library/Application Support/Claude/claude_desktop_config.json`. It preserves other servers and preferences, creates a private backup beside an existing file, then replaces the configuration atomically. A repeated identical install leaves the file unchanged. Invalid JSON, oversized files, and symbolic links are refused rather than overwritten.

Completely quit and reopen Claude Desktop afterward. The configuration location and restart requirement follow the [official local MCP setup guide](https://modelcontextprotocol.io/docs/develop/connect-local-servers).

The helper prints the backup path when it creates one. To undo setup, quit Claude Desktop and restore that backup, or remove only the `macsecurify` entry from its configuration. For an explicitly chosen configuration file, append `--config /absolute/path/to/config.json` to the setup command.

### Claude Code

With the Claude Code CLI installed:

```sh
claude mcp add --transport stdio --scope user macsecurify -- '/Applications/MacSecurify.app/Contents/Helpers/MacSecurifyMCP'
```

This uses Claude Code's own user-level setup command. Restart Claude Code, then run `/mcp` or `claude mcp get macsecurify` to check it. See [Claude Code's official MCP instructions](https://code.claude.com/docs/en/mcp).

### ChatGPT Desktop and Codex desktop

In **Security → Agent MCP**, choose your client and follow the four-slide walkthrough. ChatGPT Desktop is the default. The slides explain the purpose, local connection, setup and connection check.

Use the desktop UI without installing a CLI: open **Settings → MCP servers → Add server**, enter the server name `macsecurify`, choose **STDIO**, paste `/Applications/MacSecurify.app/Contents/Helpers/MacSecurifyMCP` into **Command**, and leave arguments empty. Save and restart. See [official OpenAI MCP setup](https://learn.chatgpt.com/docs/extend/mcp).

If you prefer Terminal, expand **Prefer Terminal?**. MacSecurify looks for the installed desktop app's bundled CLI or an installed CLI at a standard location. It copies a complete quoted executable path, so the command works without a `codex` alias or PATH entry. If no CLI is found, use the desktop settings route. The guide never runs commands or changes your client configuration automatically.

`zsh: command not found: codex` means the shell cannot find the CLI. Repeating the same bare command will not fix it. You do not need to install a CLI to use the desktop settings route. Available settings depend on the client version and workspace policy.

### Codex CLI

With the Codex CLI installed and available on PATH:

```sh
codex mcp add macsecurify -- '/Applications/MacSecurify.app/Contents/Helpers/MacSecurifyMCP'
```

Restart the local client and use `/mcp` to inspect the connection.

### ChatGPT on the web and hosted cloud chats

A cloud chat does not read this Mac's local MCP configuration or directly launch this executable. ChatGPT's custom MCP connection uses remote transports; MacSecurify does not provide a remote server or tunnel. Use a supported local client for this integration. See [OpenAI's custom MCP server connection requirements](https://developers.openai.com/api/docs/guides/custom-mcp-server).

### Another local MCP client

In a client that supports **local STDIO MCP**, set:

| Field | Value |
| --- | --- |
| Server name | `macsecurify` |
| Command | `/Applications/MacSecurify.app/Contents/Helpers/MacSecurifyMCP` |
| Arguments | None |

For clients that use the common JSON format, **Advanced setup** in MacSecurify can copy this configuration:

```json
{
  "mcpServers": {
    "macsecurify": {
      "command": "/Applications/MacSecurify.app/Contents/Helpers/MacSecurifyMCP",
      "args": []
    }
  }
}
```

Use the helper from the same installed app bundle as the running MacSecurify app. A remote-only MCP client cannot use a local executable path. Client-specific UI, administrative policies, and process launching vary; complete third-party client validation is not claimed.

## Check the connection

After restarting your AI client, ask: **“Read MacSecurify's current policy and explain which apps and websites are protected.”**

The client should appear under **Connected integrations** in MacSecurify. A connection shown as unverified may read permitted policy but cannot receive a scoped access grant. Read-only policy access needs no exception approval. Optional managed requests remain subject to the rules below.

## Agent workflow

Ask your agent to follow this sequence before accessing a resource:

1. Call `protection.get_policy` for the current rules.
2. Call `protection.check_access` for the target app bundle identifier or website origin.
3. Respect the returned decision and reason. A pending request or stale policy snapshot is not permission.
4. If the policy permits a supported exception, use `protection.request_access` and wait for native review.
5. Use `protection.get_request_status` to check the result. Only an issued grant can be passed to `protection.fetch_with_grant`.

The canonical resource is `macsecurify://policy`. The legacy `secureapp://policy` alias remains available for existing integrations. Clients that support resource subscriptions can listen for changes, then re-read the policy. Clients that do not subscribe must query again before acting.

## Modes and approval

| Mode | Behavior |
| --- | --- |
| **Notify Only** | Default mode. Exceptions are initially disabled. You can enable native request review where the policy permits it. |
| **Strict** | Rejects exception requests. |
| **Scoped Access** | Also requires explicit trust in the verified integration shown in native settings. |

Scoped access requires a directly launching, signed integration whose identity the native app can verify. Shell wrappers, interpreters, and unsigned clients can read permitted policy but cannot obtain a scoped grant. The client-supplied display name is not proof of identity.

An allowed request appears for review in MacSecurify. Notifications can open review or deny a request; they cannot approve it. Approval requires fresh **hardware Touch ID**, with no password or face fallback. If Touch ID is unavailable, the request remains blocked.

## What can be granted

The current enforceable exception is a native, credential-free **HTTPS GET** to an exact approved origin. For example:

```json
{
  "resource_type": "website",
  "resource_id": "https://example.com",
  "operation": "http_get",
  "purpose": "Read public documentation needed for this task",
  "requested_duration_seconds": 300,
  "requested_origins": ["https://example.com"]
}
```

Use the resulting grant only with `protection.fetch_with_grant` and an approved URL. The broker does not send browser cookies, stored credentials, custom headers, or follow redirects. Each response is limited to 256 KiB and 15 seconds; a timed grant permits at most 20 GETs. Returned website content remains untrusted input.

A grant does not resume an application, open a browser, change Safari rules, or permit shell access. Requests such as “let this agent use Chrome only for localhost” cannot be enforced by the app guard and return `scope_not_enforceable`.

Requests are rate limited and expire after five minutes. Grants expire or are revoked when relevant policy changes, the screen/settings lock, the adapter disconnects, the app stops, or you revoke them. Restarting the app begins without pending approvals or grants.

## Available tools

| Tool | Purpose |
| --- | --- |
| `protection.get_policy` | Read the current protection policy, mode, and supported operations. |
| `protection.check_access` | Check a target and receive an explicit access decision and reason. |
| `protection.get_recent_denials` | Read recent relevant MCP denials for this integration. |
| `protection.request_access` | Submit a bounded request for native review, where permitted. |
| `protection.get_request_status` | Check a request owned by the current adapter session. |
| `protection.cancel_request` | Cancel a pending request or revoke its capability. |
| `protection.fetch_with_grant` | Perform an approved bounded HTTPS GET through the native broker. |

MacSecurify is still a local interaction guard. An agent with broad access to your macOS account can operate outside MCP. The protocol makes cooperating agents aware and requests reviewable; it does not create a sandbox for arbitrary agent work. Read [SECURITY.md](../SECURITY.md) for the boundary.
