---
name: kontext-memory
description: Capture durable facts, decisions, and observations into Kontext as memories. Trigger when the user says to remember or save something, shares a fact with future value (a preference, price, deadline, credential-free detail), or asks what Kontext remembers. Do not trigger for secrets, incidental chat, or throwaway details.
---

# Kontext memory capture

`save_to_kontext` is the frictionless capture path: it stores a standalone
document with `kind: "memory"` without forcing the user to pick a project
first.

## What to save

- Facts with future value: decisions, preferences, constraints, prices,
  deadlines, links, names of things that recur.
- The user explicitly says "save this", "remember this", or similar.
- Observations worth keeping from the current conversation that will matter
  in another one.

## What not to save

- Passwords, verification codes, tokens, private keys, or any secret.
- Incidental personal details without an explicit request.
- Things already recorded — search first with `search_context`, and update the
  existing record instead of duplicating.

## How to save

- Call `save_to_kontext` with a concise derived title when none is supplied.
- Include a source URL when the memory came from the web or a document.
- Set an expiry timestamp when the memory is only valid until a date (a
  ticket price, a temporary state).
- Supplying `projectId` is optional; memories can be organized later.

## Recalling

- For "what do you remember about X", use `search_context` first, then
  `read_document` on the best hit.
- Memories appear in normal context/list/read tools; no special invocation is
  needed.

After saving, return the tool's Markdown title link so the user can open the
record in the Kontext Library.
