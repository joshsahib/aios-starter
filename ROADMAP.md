# ROADMAP — how this AIOS grows

The kit ships lean on purpose. A few skills, a three-bucket folder structure, essential context files. That's it. As you use it, you'll outgrow the base — this file tells you what to add, when, and why.

*The AIOS structure should look like a small, well-run life — not a hoarder's basement.*

Last reviewed: 2026-06-25

---

## What ships in the kit

| File / folder | Purpose |
|---|---|
| `CLAUDE.md` | Root operating manual. Always loaded. Edit when role, voice, or priorities change. |
| `AGENTS.md → CLAUDE.md` | Symlink enabling cross-harness parity (Claude Code, Codex, Hermes, OpenCode). |
| `SOUL.md` | Global persona. Load-bearing. Fill once, update rarely. |
| `HANDBOOK.md` | How this system works. Folder purposes, conventions, skill inventory. |
| `ROADMAP.md` | This file. What to add as you grow. |
| `journal.md` | Session diary. Append one entry per session; read when resuming after a gap. |
| `decisions.md` | Locked decisions — what was decided and why. Append-only. Never delete entries. |
| `connections.md` | Registry of every tool, MCP, and CLI your AIOS can reach. |
| `aios-intake.md` | Source of truth for `/onboard`. Edit and re-run any time to update context files. |
| `context/` | Cross-bucket shared reference: voice samples, global preferences, shared vocabulary. |
| `threads/` | Cross-bucket topic captures. One file per topic, updated as it evolves. |
| `artifacts/` | Cross-bucket generated outputs: docs, specs, scripts Claude made for you. |
| `references/` | Cross-bucket input docs: things you brought in (Codex outputs, external specs, PDFs). |
| `archives/` | Retired content — old workstreams, closed threads, deprecated skills. Move here; never delete. |
| `learnings/` | Persistent behavioral corrections and standing instructions Claude follows across all sessions. |
| `[bucket]/context/` | Domain facts: people, pricing, properties. Updated when facts change, not per session. |
| `[bucket]/threads/` | Topic captures scoped to this domain. |
| `[bucket]/workstreams/` | Active task state. One file per ongoing workstream; updated each session; archived when done. |
| `[bucket]/artifacts/` | Domain-specific generated outputs. |
| `[bucket]/references/` | Domain-specific input docs you brought in. |
| `.agents/skills/` | Canonical skills location. `.claude/skills` symlinks here. |

---

## Bucket-level CLAUDE.md — when to create one

Don't create these preemptively. Create one only when the root CLAUDE.md is causing scope pollution — getting day-job context in a business-hobby session, or vice versa. When you create one, also create the companion `AGENTS.md → CLAUDE.md` symlink in that bucket.

---

## What to add as you grow

### Folder additions

| Folder | Add when | Where |
|---|---|---|
| `[bucket]/templates/` | You catch yourself regenerating the same doc structure (spec docs, grant outlines, lease templates) | Inside the relevant bucket |
| `[bucket]/brand/` | You generate visual content consistently for a domain (venture marketing assets, work presentation slides) | business-hobby/ first; others if needed |
| `scripts/` | You write Python or Bash for tools not covered by MCP or Printing Press CLIs | Root; bucket subfolder if domain-specific |
| `[bucket]/projects/[slug]/` | A workstream exceeds 5+ files across folders, has its own team or context needs, and will run 3+ months continuously | Inside the relevant bucket |
| `.agents/agents/` | You need a sub-assistant for repeatable multi-step research or writing tasks | Root `.agents/` folder |
| Additional buckets | You develop a life domain that genuinely doesn't fit personal-family/day-job/business-hobby | Root level; same subfolder structure |

### Skill additions (see HANDBOOK.md for current inventory)

Add skills from:
- [officialskills.sh](https://officialskills.sh) — official vendor skills (Anthropic, Microsoft, Cloudflare, etc.)
- [github.com/VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) — community skills, 1K+ options
- [github.com/glebis/claude-skills](https://github.com/glebis/claude-skills) — quality knowledge-work skills

Run the Anthropic `skill-creator` skill to build custom skills. Use it when a community version doesn't exist or won't have your personal context.

| Skill type | Add when |
|---|---|
| Tool-specific (Linear, Figma, Gmail) | You wire a new tool and need structured workflows for it |
| Domain-specific custom | A community skill doesn't know your voice, context, or routing |
| Research wrappers | You find yourself repeating the same search-synthesize-cite pattern |
| Communication drafts | Email, message, or outreach patterns that should sound like you |

---

## Phase roadmap

Phases build on each other — each is worth doing only once the previous one actually works.

### Phase 1 — Foundation
Set up the folder taxonomy, populate context files via `/onboard`, and install the core skills. The system is functional when a session can start by reading context and end by writing to `journal.md`.

### Phase 2 — Tool connections
Connect your highest-frequency tools. Prefer MCP servers where they exist (calendar, email, project management). For tools without a native MCP, a lightweight CLI is often more token-efficient — frameworks like Printing Press can generate one from any web interface. Update `connections.md` after each connection.

### Phase 3 — Communication and workflow automation
Build the custom skills that require your personal context: communication drafts in your voice, action capture that routes to your task tools, brief generation that spans your domains. These won't exist in community repos — they only work because they know your specific context.

### Phase 4 — Agent layer
Add sub-agents for repeatable research or writing tasks. Evaluate which high-frequency tool connections benefit from a CLI over an MCP for token efficiency — the difference compounds across long sessions.

### Phase 5 — Local and distributed
Local LLMs for offline processing, sensitive data, or cost reduction at volume. Only pursue this when the system is mature and the use case is genuinely clear — the setup cost is real and the payoff is specific.

Use this as a guide, not a script.

---

## Suggested cadences

| File / action | Cadence |
|---|---|
| `journal.md` | Append one entry every session; read at start of next session after a gap |
| `decisions.md` | Append any time a significant choice is locked; check before major moves |
| `learnings/` | Add a file when Claude repeatedly makes the same mistake; review quarterly |
| `connections.md` | Update every time a new tool, MCP, or CLI is wired in |
| `archives/` | Quarterly cleanup — move closed workstreams, old threads, deprecated skills |
| `[bucket]/workstreams/` | Update each session that touches an active workstream; archive when done |
| `CLAUDE.md` | Quarterly review — rewrite priorities section; update after major life changes |
| `HANDBOOK.md` | Update when folder structure or skill inventory changes |
| `ROADMAP.md` | Update when a phase is complete or plans materially shift |

---

## What NOT to add

Anti-patterns that look helpful but rot the structure:

- **Don't dump raw email, Slack, or Discord archives into `references/`** — interpreted facts only; raw transcripts belong in your notes app or meeting-intelligence tool
- **Don't build folder-of-folders for organization theater** — flat with good naming beats deep nesting; if you need a hierarchy to find something, you have a search problem
- **Don't add `notes/`, `misc/`, `tmp/`, or `inbox/`** — graveyards; use `archives/` if it's old, write a real file in the right place if it's new
- **Don't pre-create folders you don't need yet** — empty folders are noise; the pain point will tell you when it's time
- **Don't have parallel decisions files** — `decisions.md` at root is the only one; no bucket-level versions
- **Don't fork your operating manual without reason** — root `CLAUDE.md` is canonical; only create bucket-level CLAUDE.md when scope pollution is a real, recurring problem
- **Don't add a `projects/` folder preemptively** — use slug-prefixed files within existing folders until a workstream genuinely earns its own subfolder (5+ files, 3+ months, distinct context needs)
- **Don't store credentials in any AIOS file** — use a secrets manager or environment variable injection; if you accidentally share a secret in chat, treat it as exposed

---

## How to tell when it's time to add a folder

Ask three questions:

1. **Is this conceptually new?** Or does it fit somewhere that already exists?
2. **Will I touch this 3+ times in the next month?** If not, it's premature.
3. **Would a future session naturally route something here?** If yes, the system will use it. If no, you're organizing for yourself, not for the AIOS.

Two yeses = add it. One yes = wait.

---

> *When you can't find something, that's a signal to consolidate — not to add another folder.*
