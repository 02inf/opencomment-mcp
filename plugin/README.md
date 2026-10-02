# OpenComment AI plugin package

This draft portable package combines a remote OpenComment MCP connection with one workflow skill. It uses root `plugin.json`, root `mcp.json`, and `skills/`, following the Agent Plugins format linked by [official OpenAI packaging documentation](https://developers.openai.com/plugins/build/plugins). The presentation/review fields sit under `extensions.com.openai`. It includes no local server, scripts, hooks, credentials, customer data, or fake connector ID.

Endpoint: `https://mcp.opencomment.ai/mcp` (Streamable HTTP). [Quickstart](docs/quickstart.md). [Real tool examples](docs/examples.md). [Skill](skills/opencomment-linkedin-workflows/SKILL.md).

The four public product tools work anonymously. Account workflows need explicit OAuth sign-in or a scoped key supplied through client settings, followed by a tool refresh. The package contains no bearer credential or permission grant. Start with company/Contact List reads without spending. Paid work requires an authorized allowance, and MCP cannot submit LinkedIn comments or start extension delivery.

This package has not been uploaded, scanned by a directory, reviewed, approved, published, or certified in a client. Schema and file validation only check packaging. The proposed five positive/three negative cases are expectations to test, not test results. A real walkthrough URL and secure reviewer access are intentionally absent. [Submission handoff](SUBMISSION.md) lists the remaining work.

To make an upload ZIP, archive this directory's contents so `plugin.json` and `mcp.json` are at the ZIP root. Include the `docs/`, `skills/`, and `assets/` folders and the README/handoff. Do not include this repository's Registry `server.json`, other client configs, Git metadata, or secret files. Use the current [OpenAI submission flow](https://developers.openai.com/plugins/deploy/submission) and complete portal checks before review.
