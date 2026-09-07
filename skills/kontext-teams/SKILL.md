---
name: kontext-teams
description: Work safely in Kontext Team workspaces. Trigger when the user names a Team, provides a Team Library link, asks about Team records or members, assigns Team work, or wants to read or change Team-owned context. Do not trigger for Personal or direct-shared records.
---

# Kontext Team workspaces

Team operations use the normal Kontext project, task, document, search, and
context tools with one explicit `workspaceRef`. Never treat a Team as a mutable
active workspace.

## Select the workspace

- If the user supplies a Kontext Library URL, `/library` path, or `rsc_`
  identity, call `open_library_link` first and use its returned `workspaceRef`.
- Otherwise call `list_workspaces`, match the Team by its returned name, and
  clarify only when the intended Team is ambiguous.
- Personal is the default when `workspaceRef` is omitted. Pass the selected
  Team's opaque `workspaceRef` on every Team read or write, and never mix IDs
  from different workspaces.
- If a resolved Library item is unavailable, do not fall back to a broader
  Personal or cross-workspace search.

## Respect capabilities

- Everyone in a Team can read Team content. Owners and admins can edit every
  item; members can edit items they created or were explicitly allowed to
  edit. Project edit access also covers its child tasks and documents.
- Use the effective permission returned with the record. Do not infer access
  from the user's request or retry a refused write against another workspace.
- Team creation, membership, and access administration are first-party Kontext
  Library actions. Point the user to https://thekontextco.ai/library rather
  than implying MCP can perform them.
- Personal Timeline, semantic relationships, saved Skills, and
  `save_to_kontext` memories do not become Team-scoped by omitting or inventing
  a `workspaceRef`.

## Make safe Team writes

- Keep the returned revision with the item. For a later update, pass it as
  `expectedRevision` when it is still the version the user reviewed. On a
  revision conflict, read the current item and reconcile instead of overwriting
  it blindly.
- Use a new `mutationId` for each logical Team write. Reuse it only when retrying
  that same write after an uncertain transport result.
- Follow the usual preview-and-confirm flow before destructive actions. Never
  use a read from one workspace as authorization to mutate another.

## Assign Team tasks

- Call `list_assignment_candidates` with the selected `workspaceRef` to obtain
  active member display names and canonical IDs without exposing email
  addresses.
- Confirm the intended assignee before passing `assigneeUserId` or
  `assignee:"me"` to `manage_task`. Pass null only when the user wants the task
  unassigned.
- Use `list_tasks.assignedTo` with `"me"`, a returned member ID, or
  `"unassigned"` to retrieve the requested work.
