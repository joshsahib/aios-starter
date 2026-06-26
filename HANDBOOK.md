# HANDBOOK — how this AIOS works

This is the reference manual for the system. If you just cloned it, read this top to bottom once and you'll understand where everything goes and why. After that, treat it as a lookup: folder purposes, naming rules, session workflows, and the skill inventory.

---

## What this is

An **AIOS** (AI Operating System) is a structured set of files and folders that gives an AI assistant durable, well-organized context about your life and work. Instead of re-explaining yourself every session, you maintain a small, well-run set of documents the assistant reads automatically. The assistant operates *on* this structure: reading context before it acts, writing decisions and session notes as it goes, and routing new material to the right place.

What makes this one different from a single-project setup is that it's organized around **buckets** — parallel life domains that run at the same time (for example: personal/family, a primary job, and an independent venture). Most AI setups assume one context. Real life doesn't work that way. Each bucket holds its own context, active work, and history, while a lean root layer holds everything cross-cutting: the global persona, the skills, and shared preferences. Context loads hierarchically — when you work inside a bucket, the assistant sees that bucket plus the root, and nothing from sibling buckets unless you pull it in explicitly.

---

## Naming conventions

Two casing rules tell you what a file is at a glance:

- **ALL CAPS = system files.** Files the assistant loads automatically or that govern how the AIOS behaves. Examples: `CLAUDE.md`, `AGENTS.md`, `SOUL.md`, `HANDBOOK.md`, `ROADMAP.md`.
- **lowercase = content files.** Files you create and maintain as you work. Examples: `journal.md`, `decisions.md`, `connections.md`, and everything inside subfolders (`threads/home-purchase.md`, `workstreams/beta-launch.md`).

**The mental test:** *Does the system read this to know how to behave, or do I write to it to record what's happening?* If it governs behavior or loads automatically, it's ALL CAPS. If it's content you produce and curate, it's lowercase.

---

## Folder reference

Each folder has one job. Keep them flat — good file naming beats deep nesting. **Bucket-level folders mirror these root-level folders but are scoped to a single domain.** A file about a domain-specific topic lives in that bucket's folder; a file that spans domains lives at root.

| Folder | What belongs | What does NOT belong | Example file |
|--------|-------------|---------------------|--------------|
| `context/` | Static domain facts — people, pricing, properties, preferences. Updated when facts change, not per session. | Session notes, active task state, anything time-bound | `context/about-me.md` |
| `threads/` | Topic captures — one file per topic, updated as it evolves across sessions | One-off task state; finished topics (archive those) | `threads/career-transition.md` |
| `workstreams/` | Active task state — one file per ongoing piece of work, updated each session, archived when done (**bucket-level only**) | Static facts; topics with no active work | `workstreams/beta-launch.md` |
| `artifacts/` | Outputs the system made for you — docs, specs, scripts, models | Things you brought in from outside (those are references) | `artifacts/launch-checklist.md` |
| `references/` | Inputs brought in from outside — external specs, partner docs, PDFs, research dumps | Things the system generated; raw chat/email transcripts | `references/partner-spec.pdf` |
| `learnings/` | Persistent behavioral corrections and standing patterns; read at session start (**root-level/global**) | Project facts, task notes, one-time instructions | `learnings/preserve-license-files.md` |
| `archives/` | Retired content — closed workstreams, old threads, deprecated material. Move here; never delete (**root-level**) | Anything still active | `archives/2026-q1-workstreams/` |

**Note:** `learnings/` and `archives/` exist only at root — behavioral rules are global, and retired content collects in one place. `workstreams/` exists only inside buckets — active task state is always domain-scoped. Every other folder appears at both levels.

---

## File reference

| File | Purpose | When to update |
|------|---------|----------------|
| `CLAUDE.md` | Root operating manual — buckets, priorities, voice, standing rules. Always loaded in every session. | When role, voice, priorities, structure, or skills change |
| `SOUL.md` | Global persona — identity, communication style, decision heuristics, confirmation rules. Load-bearing, read every session. | Rarely — only on a genuine shift in how you want the assistant to behave |
| `HANDBOOK.md` | This file — how the system works: folders, conventions, workflows, skills. | When folder structure, conventions, or the skill inventory change |
| `ROADMAP.md` | What to add as the system grows, and when. The "don't over-build" guide. | When a growth phase completes or plans materially shift |
| `journal.md` | Session diary. Root holds a brief cross-bucket summary per session; each bucket holds full domain notes. | Append one entry at the end of each session |
| `decisions.md` | Locked decisions and the reasoning behind them. Append-only — never delete entries. Root is cross-cutting; each bucket has its own. | Any time a load-bearing decision is made |
| `connections.md` | Registry of every tool, MCP, and CLI the AIOS can reach, with status and last-checked date. | Every time you wire in or change a tool |
| `aios-intake.md` | Source of truth for onboarding — your answers to the setup interview. Re-run onboarding to regenerate context files from it. | When your high-level situation changes; then re-run onboarding |

---

## How a session works

**Starting a new task**
1. Read `SOUL.md`, root `CLAUDE.md`, and `learnings/`; if inside a bucket, its `CLAUDE.md` and `context/` load too.
2. Run the `grill-with-context` skill to stress-test the plan before building.
3. Capture the thinking to `threads/` (or the relevant bucket's `threads/`).
4. Execute. For multi-session work, open or update a `workstreams/` file so the state survives.
5. End with the `retro` skill — it writes any learnings and checkpoints `journal.md`.

**Resuming after a gap**
1. Read root `journal.md` for the cross-bucket summary.
2. Read the relevant bucket's `journal.md` for full domain notes.
3. Open the relevant `workstreams/` file for exact in-flight state.
4. Resume from the documented next step.

**Cross-bucket work**
1. Start at the AIOS root (not inside a bucket) so only the lean root context loads.
2. Pull each bucket's context in explicitly — via `@include`, a skill, or a direct instruction. It is never automatic.
3. Keep outputs at root if they span domains; route domain-specific results back into the relevant bucket.

---

## Skill inventory

Skills live in `.agents/skills/` (with `.claude/skills` symlinked to it). The assistant reads a skill's `name` and `description` to judge relevance, and loads the full body only when invoking.

| Skill | What it does | When to invoke |
|-------|-------------|----------------|
| `grill-with-context` | Structured adversarial interview that stress-tests a plan against your real context before you build | Before any significant plan, design, or decision |
| `onboard` | Runs the intake interview and scaffolds the Day-1 file set | First session of a new install or bucket; re-run to refresh context |
| `audit` | Scores the setup against the Four Cs and returns the top fixes by leverage | Periodic health check; after any major structural change |
| `level-up` | Walks the 3Ms interview to find and ship one new automation | Weekly, to add one piece of leverage |
| `retro` | Scans the session for corrections and patterns, writes approved learnings, checkpoints `journal.md` | End of any substantial session |
| `handoff` | Produces a structured end-of-session handoff so a fresh session can continue seamlessly | Before clearing context mid-thread |
| `skill-creator` | Creates, improves, and evaluates skills | Building a new skill or optimizing an existing one |
| `docx` | Creates, reads, and edits Word `.docx` documents | Any Word-document task |
| `pdf` | Extracts, merges, splits, fills, OCRs, and creates PDFs | Any PDF task |
| `pptx` | Creates, reads, and edits PowerPoint `.pptx` decks | Any slide-deck task |

---

## Key conventions

- **Confirm before modifying files.** Propose a plan and get approval before writing, renaming, or deleting anything.
- **Secrets never live in files.** Use a secrets manager or environment-variable injection. If a credential shows up in chat, treat it as sensitive.
- **ALL CAPS for system files, lowercase for content.** Casing signals what a file is — keep it consistent.
- **Archive, don't delete.** Retired content moves to `archives/`. You lose nothing and keep the working tree clean.
- **Flat naming with slugs beats deep nesting.** `threads/home-purchase.md` over `threads/personal/finance/housing/home-purchase/notes.md`. If you need a hierarchy to find something, you have a search problem.
- **Create a bucket `CLAUDE.md` only when scope pollution is real.** Don't fork the operating manual preemptively — add a bucket-level one only when root context is genuinely bleeding across domains.
