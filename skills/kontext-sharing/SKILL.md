---
name: kontext-sharing
description: Work correctly with records shared to or by the user in Kontext. Trigger when the user mentions shared access, a collaborator's project, a readOnly record, or asks what they can edit in someone else's Space. Do not trigger for ordinary owned records.
---

# Kontext shared records

Records shared with the user arrive in normal MCP context, list, and read
tools with explicit capability metadata. Nothing is copied: shared content
stays in the owner's live Space.

## Reading

- Shared entities carry `access.scope`, `readOnly`, `permission`, and
  `sharedBy` provenance and use opaque recipient references.
- Treat `readOnly: true` records as reference material only.
- Shared records are read-only at the graph boundary: they cannot be
  relationship endpoints. Do not attempt `manage_context_relationship` or
  `manage_task_relationship` on them.

## Editing

- Edit a shared record only when its returned permission explicitly allows
  edit. Use the ordinary management tools with the opaque shared ID the tools
  returned — never a guessed owner-side ID.
- For an editable shared project, creating new tasks and documents inside it
  is allowed; those new items are owned by the current user, not the project
  owner.
- Never move, import, archive, delete, re-share, or administer access on
  shared records, even when edit permission is present.

## Invitations

- Sharing invitations and revocation are managed in the Kontext Library web
  UI, not via MCP. If the user wants to share or unshare something, point them
  at https://thekontextco.ai/library.
- Incoming invitations resolve through the native sharing API to a pending
  state or an accepted opaque reference; the API never exposes item metadata
  before acceptance. Do not speculate about an invitation's contents.
