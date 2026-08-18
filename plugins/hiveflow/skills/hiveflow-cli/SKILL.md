---
description: Use the Hiveflow CLI on the user's own machine — installing it, connecting an account, running local workers and cron passes, and mirroring a terminal session to the Hiveflow web and mobile apps. Use when the user asks how to install or configure the Hiveflow CLI, set up a worker locally, run the tickets pipeline, or connect a terminal session to Hiveflow.
---

# The Hiveflow CLI

The CLI (`@hiveflow/cli`, MIT) is what runs on the user's machine: it holds the
repos, the git worktrees and the local agents. The MCP tools reach the platform;
the CLI reaches the laptop. Work that touches files, branches or pull requests
happens there.

Install with `npm install -g @hiveflow/cli`, or
`curl -fsSL https://hiveflow.ai/install.sh | bash`. Entry points: `hiveflow`,
`hive`, `hf`, `flow`. Requires `bash`, `jq` and `curl`.

Verify what is installed before instructing the user — versions and available
commands change:

```bash
hiveflow --version
hiveflow -p "/help"
```

## What it does

- **Routes to AI CLIs** — infers the task type and launches the best installed
  one (Claude Code, Gemini, Codex, Aider): `/tools`, `/use`, `/route`, `/health`.
- **Runs its own agent** — a tool-use loop against the provider API with
  permissions, sessions, hooks, skills and MCP: `/agent`, `/plan`,
  `/permissions`, `/sessions`, `/mcp`.
- **Runs workers** — local agents watching kanban boards, on cron.
- **Tickets → PR** — `/tickets setup`, `/tickets`, `/fix <id>`,
  `/tickets watch`, `/tickets cron on|off|status`.
- **Remote control** — mirrors the terminal session into the Hiveflow web and
  mobile apps, so the user can keep talking to the same session from a phone.

## Non-interactive use

```bash
hiveflow -p "write tests for src/auth.js"        # router → external CLI
hiveflow agent --yes "fix the TODO in auth.js"   # native agent, auto-approve
hiveflow agent --plan "migrate auth to JWT"      # read-only exploration
hiveflow agent --json --yes "run npm test"       # structured output
hiveflow tickets watch                           # one pass, for cron/systemd
```

`--yes` auto-approves mutating tools. Without it, in `safe` mode and with no
TTY, mutating tools are rejected by design — that is the usual reason a CLI
command "does nothing" inside a script.

## Setting up a worker

`/worker add` walks through it: pick the board, pick the trigger and result
columns, write the playbook (or have it generated), choose the cadence, and
optionally enable the cron. The worker then reports a heartbeat that makes it
visible in the board's worker panel and to the `list_workers` MCP tool.

Two things worth telling the user up front: the worker only runs while that
machine is on, and its playbook is what decides quality — a vague playbook
produces vague work.

## Which side to use

Use the **MCP tools** to read and change what lives in the platform: flows,
boards, cards, worker state. Use the **CLI** when the task needs the user's
filesystem: repositories, branches, tests, pull requests. A `cli_agent` node in
a flow is the bridge — it lets a cloud flow delegate a step to that machine.
