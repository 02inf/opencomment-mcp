# OpenAI/Codex submission handoff

Status: **prepared package, not submitted**. `plugin.json` contains five proposed positive and three proposed negative test cases. None is presented as a tested result. No reviewer account, recording URL, publishing identity verification, portal proof, or tool-scan success is claimed.

The package uses the current portable layout, which discovers `mcp.json` and `skills/` at its root. Its one server is the production remote endpoint. Existing installed plugins may use the supported `.codex-plugin/plugin.json` fallback; this package uses the new root manifest and `extensions.com.openai` instead. Do not add fake `plugin_asdk_app...`, `connector_...`, or registered app IDs. A portal connection obtains its real ID separately when needed.

Before submitting for public review:

1. Choose the organization/project that should own the listing and complete its developer/business verification and required publishing permissions.
2. Upload the prepared ZIP as a new draft **with MCP**. Confirm the production URL and OAuth mode. The server initializes anonymously, so explicitly authenticate the portal test connection to expose permitted company tools.
3. Complete the portal's exact domain challenge. Host its plain-text token at the shown HTTPS origin's `/.well-known/openai-apps-challenge`; do not fabricate a token or overwrite an unrelated proof. The proof is separate from the official MCP Registry namespace verification.
4. Provide a dedicated synthetic-data review account with the company, scopes, fixtures, and access needed by the cases through the secure reviewer fields. Do not put credentials, real customer data, or reviewer login secrets in the ZIP or this repository.
5. Run all five positive and three negative cases from `plugin.json`. Record actual outputs, client/platform, version, and auth mode. Replace draft expectations with accurate review material. Also check revoked and cross-company access, retries, and untrusted text in returned posts.
6. Record a real walkthrough that shows setup, explicit sign-in/refresh, and the main workflows. Redact credentials and use synthetic or approved public data. Add its accessible URL in `extensions.com.openai.review.demo_recording_url` after the video exists, then rebuild the ZIP. No placeholder demo URL belongs in the manifest.
7. Complete skill/tool scans, fix findings, and confirm listing URLs and icons. These local schema checks are not a substitute for the portal's security/policy review. Read the current guidelines and accept any required attestations only after verifying them.
8. Submit for review. Approval and publication are separate: publish from the portal after approval, then record the actual directory URL before promoting directory availability.

Primary references: [package format](https://developers.openai.com/plugins/build/plugins), [submission and field reference](https://developers.openai.com/plugins/deploy/submission), [submission errors](https://developers.openai.com/plugins/deploy/submission-errors). They were checked on 2026-10-02 and may change.

Draft test cases are in the manifest rather than a fabricated report. Demo concepts are read-only first: review saved/candidate contact evidence, summarize stored Monitor List posts, and propose a campaign that stops before a write or paid launch. A campaign proposal is not an executed campaign; native job status and user-started extension delivery remain separate.
