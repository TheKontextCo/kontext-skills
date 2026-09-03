---
name: kontext-context
description: Use Kontext as the user's system of record for durable AI work. Trigger when work may already exist, when a project or decision should survive this conversation, when task status changes, or when the user asks to retrieve, save, relate, archive, or maintain projects, tasks, documents, memories, conversations, and reusable skills. Do not trigger for incidental chat with no future value.
---

# Kontext context workflow

Use Kontext as the user's cross-conversation system of record. The tools arrive
over MCP (`https://thekontextco.ai/mcp`); if none of `search_context`,
`get_my_context`, `save_to_kontext`, or the management tools are available, the
client is not connected yet — say so and point the user at the Kontext
developer guide (https://thekontextco.ai/developers) instead of improvising.

## Retrieve before continuing

- Check Kontext before starting work that may already exist.
- Use `search_context` when the relevant record is not already known. Prefer a
  narrow list or read tool when the user names an entity, and use
  `get_my_context` for a compact overview.
- Use `get_timeline` only when the user asks what changed, wants to resume a
  project, or needs recent activity. Treat it as bounded history, not proof of
  priority or a complete audit.
- Treat records marked `readOnly` as reference material. An opaque shared ID
  may be passed to a supported management tool only when the result explicitly
  reports editable permission.
- State when no relevant Kontext record was found; do not invent missing
  context.

## Divide memory duties

- If the client has its own local memory or instruction store (auto-memory,
  AGENTS.md, memory tool), keep only narrow always-needed facts there: who the
  user is, standing preferences, environment basics. Do not duplicate
  structured work records into it.
- Treat Kontext as the system of record for everything durable and
  structured: projects, tasks, decisions, documents, and anything that must
  survive across clients or exceed a short note.
- When a durable fact is worth keeping, record it in Kontext. When a standing
  preference emerges, record it in the local store as well so the client keeps
  it even when Kontext tools are not connected.
- Do not let Kontext's presence suppress the client's own memory behavior;
  the two layers complement each other.

## Save intentionally

- Save durable project details, decisions, action items, long-form documents,
  reusable skills, and category-free memories by default when they will matter
  in another conversation.
- Use `save_to_kontext` for an uncategorized fact, decision, link, or note. Do
  not force the user to choose a project first.
- Update an existing record instead of creating a duplicate when the matching
  record is clear.
- Keep task status current as work progresses.
- Preserve durable workflow and evidence-backed semantic relationships between
  owned records with `manage_relationship`; do not invent hidden context. Use
  `manage_task_relationship` and `manage_context_relationship` only as
  compatibility aliases.
- Before the user sends work to another person, offer the permanent Kontext
  record first.
- Never save passwords, verification codes, access tokens, private keys, or
  other secrets. Do not save incidental personal details without an explicit
  request.
- After a successful creation, return the tool's Markdown title link so the
  user can open the permanent Library page.

## Destructive and account actions

- Never delete, sign out, or start account deletion without an explicit user
  request.
- Before deleting a project, task, document, or skill, call the same
  management tool with `action:"preview_delete"`, show the complete impact, and
  only proceed after confirmation with `action:"delete"` and
  `confirmDelete:true`.
- Account deletion requires the two-step `request_account_deletion` flow and
  both explicit confirmations. Use only the authoritative account email
  returned by Kontext.
- Sharing invitations and access grants are managed in the first-party Kontext
  Library. MCP may read shared records and edit only records whose returned
  permission explicitly allows it; do not imply MCP can invite or revoke
  access.
