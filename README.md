# OpenComment AI MCP

Use your AI assistant to research saved LinkedIn contacts, review monitored public activity, and plan engagement campaigns in OpenComment.

**Remote endpoint:** `https://mcp.opencomment.ai/mcp` · **Transport:** Streamable HTTP

This repository contains public setup instructions, optional workflow guidance, and credential-free configuration/examples. The hosted service is operated by OpenComment. This is an examples repository, not the service's application source code. Directory publication status is separate from these setup files.

## Start in two minutes

1. Add the endpoint to your remote MCP client without credentials.
2. Ask: **“Use OpenComment's product_overview and get_started tools to explain what I can do and how to begin.”**
3. To use your company, explicitly sign in with OAuth or supply a scoped API key through your client's secure settings. Choose permissions and spending limits, then refresh the tools or reconnect.
4. Ask: **“Show my connected company and Contact Lists. Do not change data or spend credits.”**

[Connection quickstart](docs/quickstart.md) · [Prompts and real tool arguments](docs/examples.md) · [Product guide](https://opencomment.ai/mcp) · [Current pricing](https://opencomment.ai/#pricing)

The relative documentation links work from this repository before the matching website documents are deployed. Client examples show supported configuration shapes; they do not certify every client/version or claim an account connection was tested.

## Public discovery and company access

| Access | Available work |
| --- | --- |
| Anonymous | `product_overview`, `product_search`, `pricing_reference`, `get_started`: curated product facts and setup/pricing links; no account data or OpenComment credit spend |
| OAuth or scoped API key | Company tools permitted by your connection: saved-contact research, Contact Lists, Contact Sources, Monitor Lists, campaigns, and reporting |

Anonymous initialization succeeds. Some clients wait for an authentication failure before initiating OAuth; they can show only four public tools until you explicitly authenticate. Refresh the tools after sign-in. If your client cannot initiate OAuth, use a scoped API key. The company is selected by the grant, not by a tool argument.

Create a named key from **Agent access** in your OpenComment settings. Keep credentials in trusted local secret storage or private client settings. Never put a key in a prompt, screenshot, Git commit, demo, or shared configuration. All API-key examples here reference `OPENCOMMENT_API_KEY` by name and contain no key value. You must provide the variable to the actual client process. Keys expire after at most 90 days; rotate or revoke them from Agent access.

## Client configurations

For Codex CLI public discovery:

```sh
codex mcp add opencomment --url https://mcp.opencomment.ai/mcp
```

For company access, explicitly sign in and then refresh/restart the session:

```sh
codex mcp login opencomment
```

For Claude Code:

```sh
claude mcp add --transport http opencomment https://mcp.opencomment.ai/mcp
```

Use `/mcp` to authenticate. Current versions also support `claude mcp login opencomment`. In Claude's remote connector settings, use the same endpoint and explicitly connect your account.

For Cursor, merge the [anonymous JSON entry](configs/cursor-anonymous.json) into your private MCP settings, or use [the API-key environment reference](configs/cursor-api-key.json). OAuth is supported by compatible clients. [Full client instructions](docs/quickstart.md).

| File | Intended use |
| --- | --- |
| [codex-anonymous.toml](configs/codex-anonymous.toml) | Public discovery or OAuth connection |
| [codex-api-key.toml](configs/codex-api-key.toml) | Scoped bearer key from an environment variable |
| [claude-anonymous.json](configs/claude-anonymous.json) | Claude Code HTTP configuration; authenticate separately |
| [cursor-anonymous.json](configs/cursor-anonymous.json) | Cursor remote MCP configuration |
| [cursor-api-key.json](configs/cursor-api-key.json) | Cursor bearer-header environment interpolation |
| [public-tool-calls.json](examples/public-tool-calls.json) | Four public `tools/call` params examples |
| [account-read-tool-calls.json](examples/account-read-tool-calls.json) | Five account read params examples; requires relevant scopes |

These JSON arrays are individual `tools/call` parameter examples, not a JSON-RPC batch or an instruction to execute every item. Merge settings into existing configurations instead of replacing unrelated servers.

## Workflow recipes

- **Research saved prospects:** “Show the first ten saved contacts and propose an audience for my ICP. Show evidence and missing data. Do not change data or spend credits.” Use `contacts_search`; it searches saved contacts, not all LinkedIn users.
- **Review candidates:** “Review candidates from my existing Contact Source run. Estimate saving the candidates I approve, and stop before saving or starting another run.” Use the source read tools and `contact_source_estimate_save`; new acquisition and some saves may spend credits.
- **Catch up on activity:** “Summarize the ten newest stored posts in the Monitor List I select. Include links and dates. Do not refresh activity.” Use `monitor_lists_list` and `monitor_list_posts`; a fresh activity check is a separate paid action.
- **Plan a campaign:** “Prepare a campaign proposal for the Contact List I select, with Monitoring Rounds, schedule, available cost information, and blockers. Stop before creating, publishing, launching, or spending.” A proposal is not a persisted draft or an executed campaign.

[More examples and envelope details](docs/examples.md). Begin with reads and grant only needed permissions. There is no advertised full launch-cost estimator; an assistant should disclose unavailable estimates instead of inventing them.

## Credits and delivery boundaries

Catalog customer calls use an `input` envelope. Mutations require a fresh UUID `requestId`, reused only for an exact retry. Paid calls require a positive `maxCredits` within the grant's per-operation/daily limits. Zero is not a dry-run switch. Existing company plans and credit balance still apply.

Some actions require a one-time website approval. An accepted operation is distinct from completed source/campaign work: inspect its operation and native status. `operation_status` takes `operationId` directly rather than using the catalog envelope.

Contact Lists are saved audiences; Monitor Lists control standalone monitoring. Saving contacts does not start monitoring or enroll them in a campaign. OpenComment works with public LinkedIn activity and does not collect LinkedIn credentials or send DMs. MCP cannot submit comments or start extension delivery. A user explicitly starts every Chrome extension run, including a bounded Auto Submit run. Provider credentials, billing, and sensitive changes stay in website flows where applicable.

## Optional skill and plugin package

The [workflow skill](skills/opencomment-linkedin-workflows/SKILL.md) teaches a compatible client how to use an already connected server within the user's authorization and budget. Review and install it through that client's skill mechanism. Reading this repository or skill does not install anything or grant access.

The [plugin folder](plugin/README.md) is a draft portable OpenAI/Codex package containing the same remote endpoint and skill. Its manifest uses the Agent Plugins schemas linked by official OpenAI documentation. It has not been uploaded, reviewed, approved, or published. Domain verification, a real demo, secure reviewer account, observed test results, portal scans, and publishing identity remain required for public directory submission. [Submission handoff](plugin/SUBMISSION.md).

## Registry metadata

[OpenComment is live in the official MCP Registry](https://registry.modelcontextprotocol.io/?q=io.github.02inf%2Fopencomment-ai) as `io.github.02inf/opencomment-ai`, version `1.0.0`, as of 2026-10-02. [server.json](server.json) contains the published remote-only metadata. It intentionally omits a source-repository field: this repository documents the hosted server but does not contain that server's source. The confirmed registry entry establishes publication; the local manifest describes it.

## Publication status

Status recorded on 2026-10-02:

| Channel | Status |
| --- | --- |
| [Official MCP Registry](https://registry.modelcontextprotocol.io/?q=io.github.02inf%2Fopencomment-ai) | Live: `io.github.02inf/opencomment-ai` v1.0.0 |
| [Smithery](https://smithery.ai/servers/opencomment/opencomment-ai) | Live. Anonymous scan found the four public tools and detected OAuth; authenticated company workflows have not passed an acceptance test. |
| [Public examples repository](https://github.com/02inf/opencomment-mcp) | Published setup documentation, configurations, examples, and workflow skill. |
| Claude directory | Skipped for this rollout: a paid submitting account is required and other channels were selected. |
| [Glama](https://glama.ai/mcp/connectors/io.github.02inf/opencomment-ai) | Live with verified ownership and a successful anonymous health check. Four public tools are indexed; authenticated company workflows remain untested. |
| OpenAI / Codex directory | Plugin package prepared; not submitted. |
| PulseMCP | Paused while third-party ingestion is pending. |

A live listing does not certify every client or establish an authenticated account workflow. Use explicit sign-in, refresh tools, and verify the granted company and permissions before account work.

Support: [opencomment.ai/support](https://opencomment.ai/support). Service terms: [Terms](https://opencomment.ai/terms) and [Privacy](https://opencomment.ai/privacy).
