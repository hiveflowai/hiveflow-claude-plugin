---
description: Summarize the Hiveflow workspace — agents, approvals, flows, boards and workers — in one pass
---

Give the user a picture of their Hiveflow workspace right now.

Gather in parallel with the `hiveflow` MCP tools:

- `list_approvals` — what is waiting on the user right now
- `list_agents` — each agent's presence, and whether any is halted
- `list_flows` — how many exist, and which are active vs paused
- `list_boards` — the boards, then `get_board` on each to count cards per column
- `list_workers` on each board — which workers are connected, paused, or stale

Then report, in prose and short lists rather than a dump:

1. **Waiting on you** — pending approvals first: who asks, for what, and since
   when. This is the one section where the user is the bottleneck.
2. **Agents** — which are working, waiting or scheduled, and flag any that is
   halted (or `organizationHalted`, which stops all of them).
3. **Flows** — how many are active, and name the ones that are paused (a paused
   flow is usually unintentional).
4. **Boards** — for each, where the work is piling up: the columns with the most
   cards, and anything sitting in a human/review column waiting on a person.
5. **Workers** — which boards have a live worker, and flag any worker that is
   paused or whose last heartbeat is old, since that means nothing is being
   picked up on that board.
6. **What deserves attention** — close with the two or three things you would
   look at first, and why.

Link each item you mention with the `url` the tools return. Do not call any
tool that changes state — approving included. If a tool errors, say which one and
carry on with the rest.
