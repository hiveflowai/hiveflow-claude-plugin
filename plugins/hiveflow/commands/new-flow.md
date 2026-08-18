---
description: Design and create a Hiveflow flow from a description of what it should do
argument-hint: what the flow should do
---

Build a Hiveflow flow that does: **$ARGUMENTS**

Work in this order, and do not skip the first two steps — flows authored blind
are the ones that come back empty.

1. **Learn the account's conventions.** Call `list_flows`, pick one or two whose
   purpose is closest to this request, and read them with `get_flow`. Copy the
   node shapes and data fields that already run in this account.
2. **Agree on the design before writing it.** State in one short paragraph: what
   starts the flow, what it decides, where a human is involved if at all, and
   what it produces. If the request is ambiguous on any of those, ask — a wrong
   assumption here is cheap to fix now and expensive later.
3. **Author the graph.** Give the entry node `data.startNode: true`, wire the
   edges so every node is reachable, and grant the LLM node its tools through
   edges with `data.connectionType: "apps"` when it needs to act. Follow the
   `building-flows` skill for node types.
4. **Create it** with `create_flow`, then read it back with `get_flow` to
   confirm it was stored as intended.
5. **Do not run it** unless the user asks. If they do, warn first when the flow
   contains nodes that send email or messages or call external APIs, since those
   effects are real.

Finish by telling the user what was created, its id, and the one thing they
should check in the Hiveflow canvas before activating it.
