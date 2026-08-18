# Hiveflow plugin for Claude Code

Operate your [Hiveflow](https://hiveflow.ai) workspace from the terminal:
inspect and run visual AI workflows, work kanban boards, and supervise the
local workers that turn cards into pull requests.

## Install

```bash
/plugin marketplace add hiveflowai/hiveflow-claude-plugin
/plugin install hiveflow@hiveflow
```

On first use Claude Code opens the browser to authorize with your Hiveflow
account (email and password, Google, GitHub, or an API key). Nothing else to
configure — no keys in files.

## What you get

**A connection to your workspace** — the `hiveflow` MCP server
(`https://mcp.hiveflow.ai`, OAuth 2.1) with tools to list and run flows, read
and edit boards, and see which workers are connected. Tools that change your
account are marked so Claude asks first.

**Slash commands**

| | |
|---|---|
| `/hiveflow:status` | One-pass summary of flows, boards and workers, and what needs attention |
| `/hiveflow:new-flow <what it should do>` | Designs a flow against your account's conventions, then creates it |

**Skills** that load themselves when relevant

| | |
|---|---|
| `building-flows` | Node types, entry points, how an LLM node gets tools, what running a flow really does |
| `workers` | How columns hand work to local workers, reading comments, why a card is not picked up |
| `hiveflow-cli` | Installing and driving the `@hiveflow/cli` on your machine |

## Where each half runs

The MCP tools reach the platform — flows, boards, workers. The
[Hiveflow CLI](https://www.npmjs.com/package/@hiveflow/cli) reaches your
machine — repositories, branches, tests, pull requests. A `cli_agent` node in a
flow is the bridge between them.

If you already added Hiveflow as a connector on claude.ai, this plugin's server
is a second, separate connection used by Claude Code; both are authorized per
account and can coexist.

## Requirements

A Hiveflow account (free works). Running flows consumes execution credits, the
same as running them from the app.

## Links

- Hiveflow — https://hiveflow.ai
- API reference — https://docs.hiveflow.ai
- Help center — https://help.hiveflow.ai
- CLI — https://github.com/hiveflowai/hiveflow-cli

MIT
