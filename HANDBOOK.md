# HANDBOOK — how this AIOS works

This is the reference manual for the system. If you just cloned it, read this top to bottom once and you'll understand where everything goes and why. After that, treat it as a lookup: folder purposes, naming rules, session workflows, and the skill inventory.

---

## What this is

An **AIOS** (AI Operating System) is a structured set of files and folders that gives an AI assistant durable, well-organized context about your life and work. Instead of re-explaining yourself every session, you maintain a small, well-run set of documents the assistant reads automatically. The assistant operates *on* this structure: reading context before it acts, writing decisions and session notes as it goes, and routing new material to the right place.

What makes this one different from a single-project setup is that it is organized around **buckets** — parallel life domains that run at the same time (for example: personal life, a primary job, and an independent project). Most AI setups assume one context. Real life does not work that way. Each bucket holds its own context, active work, and history, while a lean root layer holds everything cross-cutting: the global persona, the skills, and shared preferences. Context loads hierarchically — when you work inside a bucket, the assistant sees that bucket plus the root, and nothing from sibling buckets unless you pull it in explicitly.

---

## Naming conventions

Two casing rules tell you what a file is at a glance:

- **ALL CAPS = system files.** Files that govern how the AIOS behaves or structure how sessions work. Not all of these load automatically — `CLAUDE.md` is loaded by the harness from the working directory up to root at every session start. `SOUL.md` is loaded because `CLAUDE.md` explicitly instructs it. `HANDBOOK.md` and `ROADMAP.md` are reference documents: not auto-loaded, consulted on demand by humans or the system when needed.
- **lowercase = content files.** Files you create and maintain as you work. Examples: `journal.md`, `decisions.md`, `connections.md`, and everything inside subfolders (`threads/home-purchase.md`, `workstreams/beta-launch.md`).

**The mental test:** *Does the system read this to know how to behave, or do I write to it to record what's happening?* If it governs behavior, it is ALL CAPS. If it is content you produce and curate, it is lowercase.

---

## Folder reference

Each folder has one job. Keep them flat — good file naming beats deep nesting. **Bucket-level folders mirror these root-level folders but are scoped to a single domain.** A file about a domain-specific topic lives in that bucket's folder; a file that spans domains lives at root.

| Folder | What belongs | What does NOT belong | Example file |
|--------|-------------|---------------------|--------------|
| `context/` | Static domain facts — people, priorities, preferences. Updated when facts change, not per session. | Session notes, active task state, anything time-bound | `context/about-me.md` |
| `threads/` | Topic captures — one file per topic, updated as it evolves across sessions | One-off task state; finished topics (archive those) | `threads/career-transition.md` |
| `workstreams/` | Active task state — one file per ongoing piece of work, updated each session, archived when done (**bucket-level only**) | Static facts; topics with no active work | `workstreams/beta-launch.md` |
| `artifacts/` | Outputs the system made for you — docs, specs, scripts, models | Things you brought in from outside (those are references) | `artifacts/launch-checklist.md` |
| `references/` | Inputs brought in from outside — external specs, partner docs, PDFs, research dumps | Things the system generated; raw chat or email transcripts | `references/partner-spec.pdf` |
| `learnings/` | Persistent behavioral corrections and standing patterns; read at session start (**root-level only**) | Project facts, task notes, one-time instructions | `learnings/communication-style.md` |
| `archives/` | Retired content — closed workstreams, old threads, deprecated material. Move here; never delete (**root-level**) | Anything still active | `archives/2026-q1-workstreams/` |
| `audits/` | Saved `/audit` reports — the score history the system tracks over time (**root-level only**) | Active work; anything manually created | `audits/audit-2026-07-01.md` |
| `reflects/` | Saved `/reflect` reports — the strategic history across quarters (**root-level only**) | Active work; anything manually created | `reflects/reflect-2026-Q3.md` |
| `templates/` | Reusable prompt templates you invoke when you need them. Root for cross-domain; bucket-level for domain-specific. | Outputs the system made (those are artifacts) | `templates/weekly-review.md` |

**Note:** `learnings/`, `archives/`, `audits/`, and `reflects/` exist only at root. `workstreams/` exists only inside buckets. Every other folder appears at both levels.

---

## File reference

| File | Purpose | When to update |
|------|---------|----------------|
| `CLAUDE.md` | Root operating manual — buckets, priorities, standing rules. Always loaded. | When role, priorities, structure, or bucket layout changes |
| `SOUL.md` | Global persona — communication style, decision heuristics, confirmation rules. Loaded via CLAUDE.md instruction. | Rarely — only on a genuine shift in how you want the assistant to behave |
| `HANDBOOK.md` | This file — how the system works: folders, conventions, skill inventory. Reference document, not auto-loaded. | When folder structure, conventions, or the skill inventory changes |
| `ROADMAP.md` | What to add as the system grows, and when. Reference document, not auto-loaded. | When a growth phase completes or plans materially shift |
| `journal.md` | Session diary. Root holds a brief cross-bucket summary per session; each bucket holds full domain notes. | Append one entry at the end of each session |
| `decisions.md` | Locked decisions and the reasoning behind them. Append-only — never delete entries. Root is cross-cutting; each bucket has its own. | Any time a load-bearing decision is made |
| `connections.md` | Registry of every tool, MCP, and CLI the AIOS can reach, with status and last-checked date. | Every time you wire in or change a tool |
| `aios-intake.md` | Source of truth for onboarding — your answers to the setup interview. Re-run onboarding to regenerate context files from it. | When your high-level situation changes; then re-run onboarding |

---

## How a session works

**Starting a new task**
1. Read `SOUL.md`, root `CLAUDE.md`, and `learnings/`; if inside a bucket, its `CLAUDE.md` and `context/` load too.
2. For significant plans or decisions, run the `grill-with-context` skill to stress-test assumptions before acting.
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

Skills live in `.agents/skills/` (with `.claude/skills` symlinked to it). The harness reads each skill's `name` and `description` frontmatter directly to assess relevance — it does not need to load this table. This table is the **human-readable source of truth**: update it whenever a skill is added, removed, or significantly changed. `skill-creator` and `skill-refine` both prompt you to do this automatically.

| Category | Skill | What it does | When to invoke |
|----------|-------|-------------|----------------|
| system | `onboard` | Runs the intake interview, scaffolds the Day-1 file set, and walks through SOUL.md setup | First session of a new install; re-run after editing aios-intake.md |
| system | `audit` | Scores the system against the Five Cs out of 100; identifies top gaps by leverage | Monthly; after any major structural change |
| system | `level-up` | Find/Scope/Build session to add one new capability (Mode A) or improve one that exists (Mode B) | Weekly; any time a recurring friction surfaces |
| system | `reflect` | Quarterly synthesis of audit history, learnings, and decisions; surfaces patterns and one strategic recommendation | Monthly minimum; quarterly for a full strategic review |
| session | `retro` | Scans the session for corrections and patterns; writes approved learnings; checkpoints `journal.md` | End of any substantial session |
| session | `handoff` | Produces a structured end-of-session summary so a fresh context can continue seamlessly | Before clearing context mid-thread |
| thinking | `grill-with-context` | Adversarial interview that stress-tests a plan or decision against your real context before you act | Before any significant plan, design, or decision |
| build | `skill-creator` | Builds new skills with a full draft, test, eval, and iterate loop | Building a new skill from scratch; thorough rebuild of an existing one |
| build | `skill-refine` | Targeted diagnosis and fix for a specific known problem in an installed skill | When a skill misfires, produces wrong output, or has scope drift |
| documents | `docx` | Creates, reads, and edits Word `.docx` documents | Any Word document task |
| documents | `pdf` | Extracts, merges, splits, fills, OCRs, and creates PDFs | Any PDF task |
| documents | `pptx` | Creates, reads, and edits PowerPoint `.pptx` decks | Any slide deck task |

**Skill categories** — assign every new skill to one of these when adding it to the table. If none fit, add a new category row here first.

| Category | What it covers |
|----------|---------------|
| system | Core AIOS operations: setup, health scoring, growth sessions, strategic review |
| session | Session mechanics: capturing learnings, continuing work, handing off context |
| thinking | Structured cognition before acting: brainstorming, critical analysis, adversarial pre-flight, reasoning frameworks |
| build | Creating and refining skills and agents |
| documents | Producing and editing structured files (Word, PDF, slides) |
| tools | Tool-specific workflows for connected services and CLIs |
| communication | Drafting emails, messages, and outreach in your voice |
| research | Search, synthesis, and citation workflows |

---

## Key conventions

- **Confirm before modifying files.** Propose a plan and get approval before writing, renaming, or deleting anything.
- **Secrets never live in files.** Use a secrets manager or environment-variable injection. If a credential shows up in chat, treat it as sensitive.
- **ALL CAPS for system files, lowercase for content.** Casing signals what a file is — keep it consistent. Note that ALL CAPS does not mean auto-loaded; see the naming conventions section above.
- **Archive, don't delete.** Retired content moves to `archives/`. You lose nothing and keep the working tree clean.
- **Flat naming with slugs beats deep nesting.** `threads/home-purchase.md` over `threads/personal/finance/housing/notes.md`. If you need a hierarchy to find something, you have a search problem.
- **A bucket `CLAUDE.md` contains what's operationally unique to that domain.** Create one when a bucket has genuinely distinct instructions, constraints, or integrations that differ meaningfully from root. Don't create empty ones preemptively.
- **HANDBOOK.md is the human skill inventory.** Update the skill table here whenever a skill is added, changed, or removed. The harness discovers skills directly from `.agents/skills/` — this table is for humans to browse and maintain.
