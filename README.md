# lavela

**Deploy an app from a GitHub repo to a live server, from chat.**

lavela is a remote [MCP](https://modelcontextprotocol.io) server. Your AI agent (Claude, Claude Code, Cursor, or any MCP client that supports remote servers) builds your repo on a managed server, gives you a live URL, keeps it running, and helps you operate it: logs, captured errors, health checks, managed Postgres, a custom domain you already own, email authentication, and Stripe checkout links.

> This repository contains documentation only. lavela's source code is not published here. The service runs at [console.lavela.dev](https://console.lavela.dev). lavela is in beta.

## Connect

Add this URL to your MCP client, then sign in with your lavela account when the client opens the browser (OAuth, no token to paste):

```
https://console.lavela.dev/api/mcp
```

That connection has the core tools: deploy a repo and operate it. For every tool (ads, schedules, preview environments, security scans, volumes, jobs) use:

```
https://console.lavela.dev/api/mcp/all
```

**Claude** (web, desktop, mobile): Customize → Connectors → Add custom connector → paste the URL.

**Claude Code:**

```bash
claude mcp add --transport http lavela https://console.lavela.dev/api/mcp
```

**Claude Code, Cursor, Windsurf, Claude Desktop in one step:**

```bash
npx @lavela/cli connect
```

## Install as a plugin

**Claude Code** — adds the connector and a skill that guides your agent through deploying and checking an app:

```bash
claude plugin marketplace add ym2629/lavela-mcp
claude plugin install lavela@lavela-mcp
```

In one step inside a session: `/plugin install lavela --marketplace ym2629/lavela-mcp`

**Cursor** — [add the lavela MCP server](cursor://anysphere.cursor-deeplink/mcp/install?name=lavela&config=eyJ1cmwiOiJodHRwczovL2NvbnNvbGUubGF2ZWxhLmRldi9hcGkvbWNwIn0%3D) (one-click link), or add the URL above in Cursor's MCP settings.

**Replit** — [add lavela](https://replit.com/integrations?mcp=eyJkaXNwbGF5TmFtZSI6ImxhdmVsYSIsImJhc2VVcmwiOiJodHRwczovL2NvbnNvbGUubGF2ZWxhLmRldi9hcGkvbWNwIn0%3D) (one-click link).

**Lovable** — not supported yet. Lovable's connector connects and signs in, but it rejects tool names that contain a period (lavela's tools are named like `lavela.ping`), so no tool can be used (tested 2026-10-19).

**Mistral Le Chat** — open Connectors (under Context in the current app), add a custom connector, and paste the URL above. Le Chat detects OAuth and opens lavela's sign-in page; after you allow the connection, the lavela tools are available in chat. Tested on 2026-10-19; menu names differ slightly between Le Chat versions.

The one-click links follow the formats documented by Cursor and Replit. If one does not open, add the URL manually as described under Connect.

Also from lavela: [lavela-preflight](https://github.com/ym2629/lavela-preflight), a free read-only checklist plugin for Claude Code that reviews any app before you deploy it. It does not need lavela.

## Try it

Start a new conversation and ask:

- "Check my lavela account status."
- "Analyze this repo and tell me how lavela would deploy it: https://github.com/OWNER/REPO"
- "Deploy my GitHub repo OWNER/REPO and give me the live URL."
- "Is my app up? Show me the latest logs and errors."

## What your agent can do

- **Deploy:** analyze a repo, build it from GitHub (public repos, or private repos you connect), run web apps and APIs on managed servers, rebuild after you push.
- **Data:** managed Postgres, write-only encrypted secrets, one-off jobs.
- **Operate:** logs, captured errors, health checks, start / stop / always-on controls, and a one-call overview of each project.
- **Go live:** connect a custom domain you already own (DNS only), set up email authentication (DKIM, SPF, DMARC), connect your own Stripe account and create checkout links. Your own sales settle to you in full.

The full, current tool list is always at [console.lavela.dev/llms.txt](https://console.lavela.dev/llms.txt) and [console.lavela.dev/docs/mcp](https://console.lavela.dev/docs/mcp).

## Safety

- Every tool declares whether it only reads or changes things.
- Tools that spend money or delete things tell the agent to confirm with you first, and deleting a project requires its exact name.
- Adding funds always happens in your browser on lavela's site, never through the agent.
- lavela never edits your source code.

## Use it from your own repo

Paste this into your project's `AGENTS.md` or `CLAUDE.md` so a coding agent knows how to deploy with lavela:

```markdown
## Deploying with lavela
This app can be deployed with the lavela MCP server (https://console.lavela.dev/api/mcp).
- Start with `lavela.account_status`, then run `lavela.analyze_stack` on this repo.
- Confirm any cost with the user before calling tools that spend money, such as `lavela.provision_compute_from_repo`.
- Never ask for secrets in chat or read `.env` values: use `lavela.env_link` for a local `.env` and `lavela.set_compute_secret` for single secrets.
- After deploying, `lavela.project_overview` shows URLs, server state and builds in one call.
```

## Pricing

Servers are paid from a prepaid wallet, billed by the second. New accounts may receive a limited welcome credit. Current prices: [console.lavela.dev/pricing](https://console.lavela.dev/pricing).

## Links

- Console: [console.lavela.dev](https://console.lavela.dev)
- Tool guide: [console.lavela.dev/docs/mcp](https://console.lavela.dev/docs/mcp)
- Terms: [console.lavela.dev/terms](https://console.lavela.dev/terms) · Privacy: [console.lavela.dev/privacy](https://console.lavela.dev/privacy)
- Official MCP Registry: `io.github.ym2629/lavela`

## Support

Email [support@lavela.dev](mailto:support@lavela.dev). For problems with this documentation, open an issue here.
