---
description: How Hiveflow AI agents, their approvals and their kill switch work. Use when the user asks about their agents, wants to talk to one, asks what is waiting for their approval, wants to approve or reject something, or wants to stop an agent.
---

# Hiveflow agents

An **agent** in Hiveflow is a flow shaped as `chat_input → llm → chat_output`: it
has an objective, a model, and tools wired to its LLM node (CRM, kanban,
WhatsApp, MCP skills, a computer). A **team** is one agent flow with several LLM
members that hand work to each other.

## Reading the team

- `list_agents` returns every agent of the organization with its **presence**:
  `working`, `waiting` (blocked on an approval), `scheduled`, `published`, `idle`.
  `halted: true` means the kill switch is on for that agent;
  `organizationHalted: true` means every agent is stopped.
- `get_agent` adds the objective, members, skills, apps and the agent's pending
  tasks, reminders and approvals.
- `get_agent_audit` is the record of what the agent tried to do, the policy
  decision (allowed, asked, denied) and who approved it. Use it to answer
  "why did / didn't the agent do X".

## Talking to an agent

`chat_with_agent` writes into the user's **main conversation** with the agent —
the same thread the user sees in the Hiveflow app — and waits for the reply. The
agent may use its tools while answering (send a WhatsApp, update the CRM), and
the turn consumes credits. If it returns `status: pending`, the reply will land
in the conversation; read it later with `get_agent_conversation` instead of
sending the message again.

## Approvals (human in the loop)

Flows and agents pause on actions that need a person: a discount, a message to a
new contact, a tool the user set to "ask". `list_approvals` shows what is waiting
on the user, with the action and its arguments. `respond_approval` resolves it:

- **Approving makes the action happen.** Restate what will happen and confirm
  with the user before approving — never approve on your own judgment.
- Selection requests (`selection` present) need `selected_keys`: the rows that
  continue.

## Kill switch

`halt_agents` with an `agent_id` stops one agent; without it, every agent of the
organization (requires an admin role). A halted agent keeps its conversations
but runs no tools until resumed with `halted: false`. Prefer it over pausing the
flow when an agent misbehaves: it is immediate and audited.

## Organizations and links

Agents, flows and boards live inside an organization. Tools without
`organization_id` run against the user's default one; `list_organizations` marks
it with `isDefault`, and `set_default_organization` changes it. Results include
a `url` that opens the item in the Hiveflow app in the right organization —
give the user that link instead of building one.
