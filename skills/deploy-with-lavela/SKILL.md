---
name: deploy-with-lavela
description: Use only when the user asks to put an app online, deploy a GitHub repo, or check how a live app is doing, and the lavela connector is available. Guides the agent through lavela's deploy and status tools and asks before anything that costs money or deletes something.
---

# Deploy and check an app with lavela

lavela is a remote MCP server (beta) that builds an app from a GitHub repository, runs it on a managed server, and reports on it afterwards. This skill only applies when the user has asked to deploy, host, launch or check on an app. Do not call lavela tools for anything else.

## Before you start

- If the lavela tools are not available, tell the user to connect `https://console.lavela.dev/api/mcp` in their MCP client and sign in with the browser prompt. There is no token to paste.
- Call `lavela.ping`, then `lavela.capabilities`. Capabilities lists the tools available on the current connection and the recommended flow. Tool names and flows change, so trust that output over this file.
- Call `lavela.account_status` to see whether terms have been accepted and whether the account can run servers. Explain what it says in plain words.

## Deploying a GitHub repo

1. `lavela.analyze_stack` on the repository URL. Tell the user what it detected and anything it warns about, for example data that would be lost on a server restart.
2. `lavela.launch_saas` to create the project. Follow the `next_step` hint in each result unless the user says otherwise.
3. `lavela.provision_compute_from_repo` builds the app from the repository. Poll `lavela.compute_build_status` until it finishes. If the code exists only on the user's computer, use `lavela.deploy_link`: it gives the user a one-time browser link to open.
4. When it is live, give the user the URL and say what you could and could not verify. Do not claim success before the build status says so.

## Checking on a live app

Use `lavela.project_overview` for URLs, server state and balance in one call, and `lavela.check_health` for a health check. Report what the tools returned. Logs and errors are shown on request; lavela does not monitor the app on its own.

## Rules

- Ask the user before any action that costs money or deletes something. Deleting a project requires its exact name.
- Never add funds through the agent. If the balance is too low, use `lavela.topup_link` and let the user complete it in their browser.
- Treat secret values (API keys, passwords) as sensitive: do not repeat them back or write them into files. Use lavela's secret tool, which only writes.
- lavela does not edit the user's source code. Do not promise it will.
- Payments setup depends on the countries the payment provider supports. Say so instead of promising it will work.
