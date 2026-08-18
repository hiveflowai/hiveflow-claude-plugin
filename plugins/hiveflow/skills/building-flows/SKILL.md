---
description: Build, inspect and run Hiveflow visual workflows. Use when the user asks to create a flow, add or wire nodes, explain what a flow does, run or pause a flow, or when they mention Hiveflow node types (LLM, trigger, HTTP request, memory, conditional flow, human in the loop, CLI agent, MCP tool).
---

# Building Hiveflow flows

A Hiveflow flow is a graph in ReactFlow format: `nodes` (each with `id`, `type`,
`position`, `data`) and `edges` (`id`, `source`, `target`). It is created and
operated through the `hiveflow` MCP tools: `list_flows`, `get_flow`,
`create_flow`, `run_flow`, `set_flow_state`.

## Two rules that decide whether a flow works

1. **Every flow needs an entry point.** The node where execution begins carries
   `data.startNode: true`. A flow whose nodes are all wired but has no start
   node returns an empty result — this is the single most common mistake.
2. **Execution follows the edges.** A node runs when an edge reaches it; the
   output of the source node becomes the input of the target.

Prefer inspecting a working flow with `get_flow` before authoring a new one:
copy the shapes that already run in the user's account instead of inventing
configuration.

## Node types

Most nodes use `type: "custom"` with the real kind in `data.nodeType`. Notes and
sections are their own top-level types.

**Entry and exit**
`chat_input` · `chat_output` · `form_input` · `trigger` (schedule/webhook)

**Intelligence**
`llm` (the AI agent node) · `memory` (conversation/vector memory) · `code`
(JavaScript) · `pandas_processor` · `json_builder` (transformer)

**Control flow**
`conditional_flow` · `loop` · `filter` · `merge` · `aggregate` · `wait` ·
`sub_flow` · `error_handler` · `human_in_the_loop` (pauses for a person)

**Integrations and I/O**
`http_request` · `database` · `email` · `s3_storage` · `whatsapp_input` ·
`whatsapp_output` · `extension_input` · `extension_output` · `pdf_extractor` ·
`pdf_generator`

**Agents on real machines**
`cli_agent` (the Hiveflow CLI on the user's machine) · `mac_agent` ·
`pc_agent` · `linux_agent`

**MCP**
`mcp_server` · `mcp_client` · `mcp_tool` · `mcp_resource`

**Presentation**
`chart` · `data_table` · `html_view` · `hiveapp` · `inspector` · `note` ·
`section`

## Giving an LLM node tools

An edge whose `data.connectionType` is `"apps"` does not pass data — it grants
capability. Wiring a kanban app, an MCP tool or a `cli_agent` node into an `llm`
node that way turns it into a tool the model can call during the run. This is
how a flow becomes an agent that acts instead of just answering.

## Running flows

`run_flow` executes with the flow's saved configuration and returns per-node
results. It consumes execution credits and its side effects are real: a flow
containing `email`, `whatsapp_output` or `http_request` nodes will send and
write. Confirm with the user before running a flow whose nodes reach the
outside world, and prefer a read-only flow when demonstrating.

`set_flow_state` with `paused` stops scheduled and triggered runs without
deleting anything; `active` resumes them.

## Reporting back

When explaining a flow, describe what it does in the user's terms — its trigger,
the decisions it makes, where a human is involved and what it produces — rather
than listing node ids.
