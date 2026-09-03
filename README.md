# Kontext Skills

Official [Agent Skills](https://agentskills.io) for [Kontext](https://thekontextco.ai) — the tool-neutral context layer for AI clients. These skills teach any SKILL.md-compatible agent, IDE, or LLM client how to use Kontext as the user's cross-conversation system of record.

The tools arrive over MCP at `https://thekontextco.ai/mcp`. The skills teach the model *when and how* to use them: retrieving context before starting work, running a task loop, capturing memories, and respecting shared-access boundaries.

## Skills

| Skill | Use it for |
|---|---|
| [`kontext-context`](skills/kontext-context/) | Core workflow: retrieve before working, save intentionally, safe destructive actions |
| [`kontext-tasks`](skills/kontext-tasks/) | The task loop: check what exists → track → update → close → decide what's next |
| [`kontext-memory`](skills/kontext-memory/) | Capture discipline: what is worth `save_to_kontext`, what must never be saved |
| [`kontext-sharing`](skills/kontext-sharing/) | Working with shared records: readOnly boundaries, edit permissions, invitations |

## Install

Connect your client to Kontext over MCP first (see the [developer guide](https://thekontextco.ai/developers)), then install the skills into your agent's skills directory.

### One command

```bash
npx skills add TheKontextCo/kontext-skills
```

The [skills CLI](https://github.com/vercel-labs/skills) detects your installed agents and places the skills correctly.

### Manual

Copy each skill folder into the directory your agent reads:

| Agent | Project scope | Personal scope |
|---|---|---|
| Codex CLI | `.agents/skills/<name>/` | `~/.agents/skills/<name>/` |
| Claude Code | `.claude/skills/<name>/` | `~/.claude/skills/<name>/` |
| Cursor | `.cursor/skills/<name>/` | `~/.cursor/skills/<name>/` |
| Gemini CLI | `.gemini/skills/<name>/` | `~/.gemini/skills/<name>/` |
| OpenCode | `.agents/skills/<name>/` | `~/.agents/skills/<name>/` |
| Hermes | `~/.hermes/skills/<name>/` | — |

`.agents/skills/` is the vendor-neutral canonical path; several agents scan it as a fallback. Start a fresh session after installing so the agent rescans its skills.

## Requirements

- A Kontext account: sign up through any MCP client authorization at `https://thekontextco.ai/mcp`.
- A client that supports both MCP and SKILL.md. Clients that speak MCP but not skills (e.g. chat surfaces) still get Kontext behavior from the server's own MCP instructions — skills are the power-user layer.

## Compatibility

`kontext-context` also ships inside [`pi-kontext`](https://www.npmjs.com/package/pi-kontext) for the [pi coding agent](https://pi.dev). pi users should install via `pi install npm:pi-kontext`; this repository is the client-neutral source of truth for the same guidance.

## Validation

Frontmatter is validated in CI against the Agent Skills spec. To validate locally:

```bash
npx skills-ref validate skills/*/SKILL.md
```

## License

[MIT](LICENSE)
