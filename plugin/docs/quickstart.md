# Connect your AI assistant to OpenComment

Research saved LinkedIn contacts, review monitored public activity, and prepare engagement campaigns from your assistant. Start with public product information, then connect a company when you want to use your account.

| Setting | Value |
| --- | --- |
| Server URL | `https://mcp.opencomment.ai/mcp` |
| Transport | Streamable HTTP |
| Public discovery | No authentication or account required |
| Company access | OAuth browser sign-in or a scoped API key |
| Product guide | https://opencomment.ai/mcp |
| Current pricing | https://opencomment.ai/#pricing |

## Get your first result

1. Add the server URL to a client that supports remote HTTP MCP. Leave authentication fields empty for public discovery.
2. Ask: **“Use OpenComment's product_overview and get_started tools to explain what I can do and how to begin.”**
3. You should see these four public tools: `product_overview`, `product_search`, `pricing_reference`, and `get_started`. They return curated product facts and links, not company data or general LinkedIn search results. Public discovery does not spend OpenComment credits.

For account workflows, explicitly sign in to the same connection or configure an API key, refresh the tools, and ask: **“Show my connected company and Contact Lists. Do not change data or spend credits.”** The expected account tools are `company_get` and `contact_lists_list`, if you allowed their read permissions. [More prompts and tool arguments](examples.md).

## Connect your company

With OAuth, use your client's **Authenticate**, **Sign in**, or equivalent action. Choose your company and review permissions, expiry, and spending limits in the OpenComment consent screen. Read permissions are selected by default and spending starts at zero. Allow only the access you need.

**Refresh the tools or reconnect after sign-in.** Anonymous initialization succeeds, so a client that waits for a 401 before starting OAuth can show only the four public tools. Explicit sign-in solves that when the client supports it. If it cannot initiate OAuth, use an API key. You can sign up during the browser flow and return to the connection.

To create an API key, open **Agent access** in OpenComment account settings, choose your company, and create a named key with the needed permissions. Keys expire after at most 90 days and are only displayed when created. Provide the key through your client's secure credential settings or an environment variable as a bearer token. Never put it in a chat prompt, screenshot, repository, public document, or shared configuration.

The OAuth issuer is `https://api.opencomment.ai/auth`; discovery uses standard metadata and authorization-code sign-in with PKCE. Review or revoke a connection from **Agent access**. Every company connection has its own authorization boundary.

## Codex

Add an anonymous connection in the CLI:

```sh
codex mcp add opencomment --url https://mcp.opencomment.ai/mcp
```

For company access, explicitly run OAuth sign-in:

```sh
codex mcp login opencomment
```

Then start a new session or refresh the client so it loads the authorized tools. In Codex's setup form, choose Streamable HTTP and use the same URL. For an API key, set **Bearer token env var** to `OPENCOMMENT_API_KEY`: that field takes the variable's name, not the key. The running Codex process must have that variable in its environment.

For CLI API-key access, first provide `OPENCOMMENT_API_KEY` through your trusted local secret manager or shell environment, then use this command instead of the anonymous add command:

```sh
codex mcp add opencomment --url https://mcp.opencomment.ai/mcp --bearer-token-env-var OPENCOMMENT_API_KEY
```

Equivalent configuration in `~/.codex/config.toml`:

```toml
[mcp_servers.opencomment]
url = "https://mcp.opencomment.ai/mcp"
bearer_token_env_var = "OPENCOMMENT_API_KEY"
```

For public discovery or OAuth, omit `bearer_token_env_var`. Do not configure a literal bearer header alongside the environment-variable option. If using the form's **Headers** option instead, set `Authorization` to `Bearer <your key>` in that private setting only. Desktop apps do not necessarily inherit a terminal's environment; use OAuth or the app's private header setting if that variable is unavailable. [Official Codex MCP documentation](https://learn.chatgpt.com/docs/extend/mcp?surface=cli).

## Claude

In Claude's connectors settings, add a custom remote connector using the server URL. For account workflows, explicitly connect/sign in and then refresh the available tools. Availability depends on your Claude plan and workspace settings.

For Claude Code:

```sh
claude mcp add --transport http opencomment https://mcp.opencomment.ai/mcp
```

Use `/mcp` inside Claude Code to authenticate. Current Claude Code versions also support:

```sh
claude mcp login opencomment
```

If your installed version does not support that command, use `/mcp`. An anonymous “Connected” status only confirms public access. [Official Claude Code MCP documentation](https://code.claude.com/docs/en/mcp).

## Cursor

Add this to your private global `~/.cursor/mcp.json`, or merge the `opencomment` entry into an existing `mcpServers` object:

```json
{
  "mcpServers": {
    "opencomment": {
      "url": "https://mcp.opencomment.ai/mcp"
    }
  }
}
```

For account access, authenticate with Cursor's OAuth action and refresh the tools. If that action is unavailable, use the API-key form below. Set `OPENCOMMENT_API_KEY` in the environment available to Cursor; the JSON contains only a variable reference:

```json
{
  "mcpServers": {
    "opencomment": {
      "url": "https://mcp.opencomment.ai/mcp",
      "headers": {
        "Authorization": "Bearer ${env:OPENCOMMENT_API_KEY}"
      }
    }
  }
}
```

Cursor's remote connections support environment interpolation in headers, but not `envFile`. Restart or reconnect after changing its environment. [Official Cursor MCP documentation](https://cursor.com/docs/mcp).

## Credits and control

- Read saved company data first. Paid Contact Source work, saving some acquired contacts, activity checks, and campaign launches need spending permissions and positive limits.
- Ask for the audience, proposed action, cost information, blockers, and maximum credits before paid work. Not every tool provides a full launch estimate; do not invent one.
- Catalog account calls wrap ordinary fields in `input`; the company comes from the connection. Mutations need a fresh UUID `requestId`, reused only for an exact retry. Paid tools also require a positive `maxCredits` within the grant's per-operation and daily limits. Zero is not a dry-run flag: do not call a paid tool for a preview.
- Some actions return an approval link. Review the exact action in OpenComment, then retry the same request and arguments. An accepted operation is not proof that its underlying source run or campaign finished; use its status tool.
- Contact Lists hold saved campaign audiences. Monitor Lists control standalone monitoring. Saving contacts does not start monitoring or enroll them in a campaign.
- MCP prepares and manages workflows. It cannot submit LinkedIn comments, send DMs, or start Chrome extension posting. A user explicitly starts each extension delivery run. Billing, sensitive account changes, and provider credentials remain in website flows where applicable.

## If something fails

Only public tools after sign-in: explicitly authenticate, check selected permissions, and refresh the tool list or start a new session. A valid connection only advertises account tools permitted by its grant.

401 with a configured token: check expiry and revocation, or remove the token to return to public discovery. Invalid credentials fail even for public calls. For paid work, also check company entitlement, credit balance, spending permission, and selected limits.

Use the exact `/mcp` endpoint above; the old `/public` endpoint is retired. Client versions and menus change. These instructions describe supported configuration shapes, not certification of every client/version. [Support](https://opencomment.ai/support).
