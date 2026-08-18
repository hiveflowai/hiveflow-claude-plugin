---
description: Summarize the Hiveflow workspace — flows, boards and workers — in one pass
---

Give the user a picture of their Hiveflow workspace right now.

Gather in parallel with the `hiveflow` MCP tools:

- `list_flows` — how many exist, and which are active vs paused
- `list_boards` — the boards, then `get_board` on each to count cards per column
- `list_workers` on each board — which workers are connected, paused, or stale

Then report, in prose and short lists rather than a dump:

1. **Flows** — how many are active, and name the ones that are paused (a paused
   flow is usually unintentional).
2. **Boards** — for each, where the work is piling up: the columns with the most
   cards, and anything sitting in a human/review column waiting on a person.
3. **Workers** — which boards have a live worker, and flag any worker that is
   paused or whose last heartbeat is old, since that means nothing is being
   picked up on that board.
4. **What deserves attention** — close with the two or three things you would
   look at first, and why.

Do not call any tool that changes state. If a tool errors, say which one and
carry on with the rest.
