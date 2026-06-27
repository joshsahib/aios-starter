# ROADMAP — how this AIOS grows

The kit ships lean on purpose. A few skills, a folder structure, essential context files. As you use it, you'll outgrow the base — this file tells you what to add, when, and why.

*The AIOS structure should look like a small, well-run life — not a hoarder's basement.*

Last reviewed: 2026-06-26

---

## What ships in the kit

| File or folder | Purpose |
|---|---|
| `CLAUDE.md` | Root operating manual. Always loaded. Edit when role, priorities, or bucket structure changes. |
| `AGENTS.md → CLAUDE.md` | Symlink enabling cross-harness parity. |
| `SOUL.md` | Global persona. Load-bearing. Set up during onboarding, update rarely. |
| `HANDBOOK.md` | How this system works. Folder purposes, conventions, skill inventory. |
| `ROADMAP.md` | This file. What to add as you grow. |
| `journal.md` | Session diary. Append one entry per session; read when resuming after a gap. |
| `decisions.md` | Locked decisions — what was decided and why. Append-only. |
| `connections.md` | Registry of every tool, MCP, and CLI your AIOS can reach. |
| `aios-intake.md` | Source of truth for `/onboard`. Edit and re-run any time to update context files. |
| `context/` | Cross-bucket shared reference: voice samples, global preferences, shared vocabulary. |
| `threads/` | Cross-bucket topic captures. One file per topic, updated as it evolves. |
| `artifacts/` | Cross-bucket generated outputs: docs, scripts, and other things the system made for you. |
| `references/` | Cross-bucket input docs: things you brought in from outside. |
| `archives/` | Retired content. Move here; never delete. |
| `learnings/` | Persistent behavioral corrections the system follows across all sessions. |
| `audits/` | Saved `/audit` reports. Created by `/audit` when you choose to save. Tracks score over time. |
| `reflects/` | Saved `/reflect` reports. Created by `/reflect` when you choose to save. Tracks strategy over time. |
| `templates/` | Reusable prompt templates. Empty at install; populated as you build them. |
| `[bucket]/context/` | Domain facts: people, priorities, properties. Updated when facts change, not per session. |
| `[bucket]/threads/` | Topic captures scoped to this domain. |
| `[bucket]/workstreams/` | Active task state. One file per ongoing workstream; updated each session; archived when done. |
| `[bucket]/artifacts/` | Domain-specific generated outputs. |
| `[bucket]/references/` | Domain-specific input docs you brought in. |
| `.agents/skills/` | Canonical skills location. `.claude/skills` symlinks here. See `HANDBOOK.md` for the skill inventory. |

---

## Bucket-level CLAUDE.md — when to create one

A bucket-level `CLAUDE.md` contains operational instructions specific to that domain — unique constraints, integrations, or behavioral rules that differ meaningfully from the root. Create one when a bucket has genuinely distinct operating context. When you create one, also create the companion `AGENTS.md → CLAUDE.md` symlink in that bucket.

---

## What to add as you grow

### Folder additions

| Folder | Add when | Where |
|---|---|---|
| `[bucket]/templates/` | Domain-specific prompt templates that would clutter root templates/ | Inside the relevant bucket |
| `[bucket]/brand/` | You generate visual content consistently for a domain | business or venture bucket first |
| `scripts/` | You write scripts for tools not covered by MCP or Printing Press CLIs | Root; bucket subfolder if domain-specific |
| `[bucket]/projects/[slug]/` | A workstream exceeds 5+ files, has its own team or context needs, and will run 3+ months continuously | Inside the relevant bucket |
| `.agents/agents/` | You need a sub-assistant for repeatable multi-step research or writing tasks | Root `.agents/` folder |
| Additional buckets | You develop a life domain that genuinely does not fit your current structure | Root level; same subfolder layout |

### Skill additions

The skill inventory lives in `HANDBOOK.md` — update it any time a skill is added, removed, or significantly changed. `skill-creator` and `skill-refine` both prompt you to do this automatically.

Skills worth adding as your system matures:

| Skill type | Add when |
|---|---|
| Tool-specific workflows | You wire a new tool and want structured, repeatable patterns for using it |
| Communication drafts | You write similar emails, messages, or outreach often enough that a template would save real time |
| Research wrappers | You run the same search-synthesize-summarize pattern repeatedly across sessions |
| Domain-specific custom | A community skill does not know your context, voice, or routing well enough to be useful |

Find skills at:
- The Anthropic skills repo and official vendor skills
- https://www.skills.sh
- https://claudeskills.info
- https://github.com/VoltAgent/awesome-agent-skills
- https://github.com/garrytan/gstack
- Community skill directories for knowledge-work and productivity
- `skill-creator` — to build and test your own when a community version does not exist or fit

---

## Phase roadmap

Phases build on each other — each is worth doing only once the previous one actually works.

### Phase 1 — Foundation
Set up the folder structure, populate context files via `/onboard`, and install the core skills. The system is functional when a session can start by reading context and end by writing to `journal.md`. Run `/audit` at the end of Week 1 to get a baseline score.

### Phase 2 — Tool connections
Connect your highest-frequency tools. Check the Printing Press library first (https://github.com/mvanhorn/printing-press-library/tree/main/library) — it has 50+ pre-built CLIs that are significantly more token-efficient than MCP servers for agent use. For tools without a Printing Press CLI, use MCP servers as the next option. Update `connections.md` after each new connection.

### Phase 3 — Communication and workflow automation
Build the custom skills that require your personal context: communication drafts in your voice, action capture that routes to your task tools, brief generation that spans your domains. These won't exist in community repos — they work because they know your specific situation.

### Phase 4 — Agent layer
Add sub-agents for repeatable research or writing tasks. At this stage, evaluate whether high-frequency tool connections benefit from a CLI over an MCP for token efficiency — the difference compounds across long sessions.

### Phase 5 — Local and distributed
Local LLMs for offline processing, sensitive data, or cost reduction at volume. Only pursue this when the system is mature and the use case is genuinely clear — the setup cost is real.

Use this as a guide, not a script.

---

## Suggested cadences

| File or action | Cadence |
|---|---|
| `journal.md` | Append one entry every session; read at start of next session after a gap |
| `decisions.md` | Append any time a significant choice is locked; check before major moves |
| `learnings/` | Add a file when the system repeatedly makes the same mistake; review quarterly |
| `connections.md` | Update every time a new tool, MCP, or CLI is wired in |
| `archives/` | Quarterly cleanup — move closed workstreams, old threads, deprecated skills |
| `[bucket]/workstreams/` | Update each session that touches an active workstream; archive when done |
| `CLAUDE.md` | Quarterly review — update priorities and bucket layout after major life changes |
| `HANDBOOK.md` | Update the skill inventory whenever a skill is added, changed, or removed |
| `ROADMAP.md` | Update when a phase is complete or plans materially shift |
| `/audit` | Monthly; save the report to `audits/` each time to track score trends |
| `/reflect` | Quarterly; save the report to `reflects/` to track strategic patterns over time |

---

## What NOT to add

Anti-patterns that look helpful but rot the structure:

- **Don't dump raw email, Slack, or Discord archives into `references/`** — interpreted facts only; raw transcripts belong in your notes app or meeting-intelligence tool
- **Don't build folder-of-folders for organization theater** — flat with good naming beats deep nesting; if you need a hierarchy to find something, you have a search problem
- **Don't add `notes/`, `misc/`, `tmp/`, or `inbox/`** — graveyards; use `archives/` if it is old, write a real file in the right place if it is new
- **Don't pre-create folders you don't need yet** — empty folders are noise; the pain point tells you when it is time
- **Don't fork your operating manual without reason** — root `CLAUDE.md` is canonical; only create bucket-level CLAUDE.md when scope pollution is a real, recurring problem
- **Don't add a `projects/` folder preemptively** — use slug-prefixed files within existing folders until a workstream genuinely earns its own subfolder (5+ files, 3+ months, distinct context needs)
- **Don't store credentials in any AIOS file** — use a secrets manager or environment variable injection; if you accidentally share a secret in chat, treat it as exposed
- **Don't create empty bucket CLAUDE.md files** — only add one when a bucket has genuinely distinct operational context that differs from root. Root `CLAUDE.md` handles the rest.

---

## How to tell when it is time to add a folder

Ask three questions:

1. **Is this conceptually new?** Or does it fit somewhere that already exists?
2. **Will I touch this 3+ times in the next month?** If not, it is premature.
3. **Would a future session naturally route something here?** If yes, the system will use it. If no, you are organizing for yourself, not for the AIOS.

Two yeses = add it. One yes = wait.

---

> *When you can't find something, that is a signal to consolidate — not to add another folder.*
