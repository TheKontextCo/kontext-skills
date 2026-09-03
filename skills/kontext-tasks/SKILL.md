---
name: kontext-tasks
description: Run work through Kontext's task loop. Trigger when the user starts a non-trivial task, asks to track or resume work, reports progress or a blocker, finishes something, or asks what to work on next. Do not trigger for one-off questions with no durable outcome.
---

# Kontext task loop

Carry work through Kontext end to end: check what already exists, track the
task, keep it current, close it out, then decide what is next. Requires the
Kontext MCP tools (see `kontext-context`).

## 1. Before work

1. Call `get_my_context` and `search_context` for anything relevant to the
   task. Read what is relevant before planning.
2. If a matching open task exists, adopt it. Otherwise create one with
   `manage_task` and set it `in_progress`.
3. If the task belongs to a project, link it with `parentId`. If it depends on
   or blocks other tasks, record that with `manage_task_relationship`.

## 2. During work

- Update the task at real milestones only: a decision, a blocker, a finished
  phase. Do not spam status notes.
- When a decision is made or durable context emerges, save it
  (`save_to_kontext` or `manage_document`) and keep the task record current.
- If work stalls on something external, record the blocker so the next session
  can pick it up.

## 3. After work

1. Mark the task `done` with an outcome summary in its description.
2. Save the durable learnings that will matter in another conversation.
3. Propose the two or three best next steps from the user's open tasks, then
   stop and wait for the user to choose. Never start the next task unprompted.

## What next

When the user asks what to work on next: list open tasks with
`list_tasks`, respect `blockedByIds` and priorities, and present a short,
concrete recommendation with reasons — do not begin it without confirmation.
