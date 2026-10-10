---
description: Review what your Hiveflow flows and agents are waiting on you to approve, and decide each one
---

Walk the user through their pending Hiveflow approvals.

1. Call `list_approvals`. If there are none, say so in one line and stop.
2. For each request, oldest first, show: who asks (agent or flow, with its
   `url`), what action it wants to take and with which arguments, any fields or
   files attached, and when it expires. For a selection request, show the rows.
3. Ask the user for a decision on each one — approve, reject, or skip — with an
   optional comment. Do not decide for them, and do not batch-approve unless they
   explicitly say so.
4. Apply each decision with `respond_approval` (with `selected_keys` for
   selection requests). Approving makes the action happen, so restate it in the
   confirmation.
5. Finish with what was approved, rejected and left pending.
