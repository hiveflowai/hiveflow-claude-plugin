---
description: Work with Hiveflow kanban boards and the local workers that pick up cards. Use when the user asks about their boards, cards or columns, wants to hand work to a worker, asks why a card is not being picked up, or mentions the tickets-to-PR pipeline, playbooks or human-in-the-loop columns.
---

# Boards and workers

A Hiveflow board is a kanban app instance: columns plus cards. A **worker** is
an agent running on the user's own machine (through the Hiveflow CLI) that
watches one column of one board and works its cards according to a playbook the
user wrote, on a cron cadence.

Available tools: `list_boards`, `get_board`, `create_card`, `move_card`,
`comment_card`, `list_workers`.

## How work is handed over

The column is the interface. A worker watches a trigger column; moving a card
into it is the act of assigning work. The worker then walks the card through:

```
trigger (To Do) → working (In Progress) → review (QA) → human (needs a person)
```

A worker never moves a card past review or the human column — shipping to
production stays a human decision. Nothing is assigned by naming a person; it is
assigned by moving the card.

Because of this, `move_card` can start real work on the user's machine (writing
code, opening pull requests). Say what will likely happen before moving a card
into a watched column.

## Reading the state of a board

`get_board` returns columns and cards flattened with their `column`. Cards carry
the fields the user configured — typically title, description, priority, and
often comments and attachments.

**Comments are the feedback channel.** When a worker leaves a card in the human
column, the reason is usually in a comment; when the user replies with a comment
and moves the card back, the worker reads that history and retries. Use
`comment_card` to answer a worker's question or to leave context for the next
run, and read existing comments before concluding why a card stalled.

## When a card is not being picked up

Check in this order, and report which one it was:

1. `list_workers` — is a worker connected to that board at all, and when did it
   last check in? A stale heartbeat means the CLI is not running on the machine.
2. Is that worker `paused`? Paused workers stay listed but take nothing.
3. Is the card in the column the worker actually watches? A card in the wrong
   column is invisible to it.
4. Has the card already exhausted its attempts and been parked in the error or
   human column? Its comments will say so.

Workers run on the user's machine, so the answer is often "the CLI is not
running there", not something to fix in Hiveflow.

## The DevOps pipeline

The built-in specialist worker (`/tickets` in the CLI) is the one that turns a
support ticket into a pull request: it triages the ticket, works in an isolated
git worktree, requires a reproduction test, blocks diffs that touch secrets or
CI, waits for CI to be green, and only then moves the card to approval. General
workers are the same machinery with a user-written playbook instead of that
fixed pipeline.
