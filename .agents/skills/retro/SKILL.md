---
name: retro
description: Post-session retrospective. Scans the conversation for corrections, patterns, and carry-forward rules, then writes approved learnings to the right AIOS files and checkpoints journal.md. Use when the user says "/retro", "let's do a retro", "wrap up", "session review", or at the end of any substantial session.
---

# Retro

Interactive post-session retro. Scans the current conversation, asks focused questions, proposes concrete actions the user approves in one step. Two moments: one question call, then silent execution.

---

## Gate check (silent)

Estimate session depth from the conversation:

- **Short** (~1-2 tasks, minimal back-and-forth) → **Fast mode**
- **Substantial** (multiple tasks, corrections, errors, skill invocations) → **Full mode**

---

## Full mode

Silently scan the conversation and collect:

1. **User corrections** — explicit "no, do it this way" moments (highest signal)
2. **Skills invoked** — which succeeded, which failed, workarounds applied
3. **Repeated patterns** — same error or same workaround more than once
4. **Workflow patterns** — 3+ steps chained, or a process worth documenting
5. **Active task** — was a concrete named task being worked on? If yes, identify: task slug (kebab-case, e.g. `phase-3-structure`), target bucket (check root CLAUDE.md for active bucket names), and a summary of current state, open questions, blockers, and the exact next step

Then read existing state before generating candidates:
- Read `MEMORY.md` and any relevant memory files for this project
- Read `.agents/skills/{name}/SKILL.md` for any skills referenced in this session
- If an active task was detected, check whether `{bucket}/workstreams/{slug}.md` already exists

Generate up to **5 candidate actions**, ranked by signal strength:
1. User corrections (highest priority)
2. Failed or workarounded skills
3. Repeated patterns
4. Task doc create/update — if an active task was detected, always include this
5. Workflow patterns (lowest)

**Dedup rule:** Drop any candidate whose content already exists in the target file. For task docs, update in place rather than dropping.

Present in a **single AskUserQuestion call** (up to 4 questions):

| # | Question | Type |
|---|----------|------|
| 1 | "Quick session check?" | Single select: `Productive / Mixed / Rough / Skip retro` |
| 2 | "What felt slow or broken?" | Free text via Other (optional) |
| 3 | "Anything to carry forward as a rule?" | Free text via Other (optional) |
| 4 | "Which of these should I save?" | Multi-select from candidates. Always include "Nothing / skip all." |

If Q1 = "Skip retro" → exit immediately, but still append journal.md checkpoint.
If Q1 = "Rough" and Q2/Q3 empty → exit with "Nothing to save — session closed." Still append journal.md.

---

## Fast mode

Single AskUserQuestion call:
- "Anything worth remembering from this session?" → options: "Nothing, we're done" / Other (free text)

If "Nothing" → append journal.md checkpoint, exit.
If free text → save as feedback memory, then append journal.md checkpoint, exit.

---

## Execute (silent, no re-confirmation)

For each approved candidate (plus any insight from Q2/Q3 free text):

1. Read the target file before writing
2. Check for conflicts or duplicates against current content
3. Write if clean; skip with a one-line warning if conflict detected

**Action targets:**

| Type | Target |
|------|--------|
| Feedback memory | Auto-memory directory (path specified in session context) — `feedback_*.md` + update `MEMORY.md` index |
| Project memory | Auto-memory directory (path specified in session context) — `project_*.md` + update `MEMORY.md` index |
| Skill update | `.agents/skills/{name}/SKILL.md` |
| Bucket CLAUDE.md rule | `{bucket}/CLAUDE.md` (bucket names from root CLAUDE.md) |
| Root CLAUDE.md rule | `CLAUDE.md` |
| Load-bearing decision | `decisions.md` — find at root or nearest parent; use only for decisions where the WHY would still matter in 18 months |
| Task doc | `{bucket}/workstreams/{slug}.md` — current state, open questions, blockers, exact next step |

**Task doc format** (create or update in place):

```markdown
# {Task Name}

**Bucket:** {bucket} | **Updated:** YYYY-MM-DD

## Current state
{What's done and working}

## Open questions
{Unanswered questions, or "None"}

## Blockers
{What's blocking progress, or "None"}

## Exact next step
{One concrete action — specific enough to resume without re-reading the thread}
```

**Always last — append journal.md checkpoint** (find at root or nearest parent):

```
## YYYY-MM-DD — {session topic or goal}
- {what was decided or completed}
- {what's in flight}
- Next: {next step}
```

---

## Summary (brief)

One line per action taken:

```
Updated grill-with-context skill — added confirmation bias check note
Saved feedback memory — prefer inline edits over summary narration
Appended journal.md checkpoint
Skipped: audit skill update (already documented)
```

Done. No trailing commentary.

---

## Multi-session mode (explicit opt-in)

Invoke with `/retro today` or `/retro YYYY-MM-DD` to scan all of today's sessions across projects.

Discover JSONL session files:
```bash
find ~/.claude/projects -maxdepth 2 -name "*.jsonl" -not -path "*/subagents/*" -mtime 0
```

For each file found: extract the project name from the path, pull the first 10 and last 5 user messages to build a condensed transcript, skip sessions with fewer than 3 user messages. Present the session list before proceeding.

Then run Full mode across all sessions combined. Each candidate should note which session (project name + short session ID) it came from.

---

## Rules

- Never write learnings into this skill file itself — distribute to skills or memory
- Cap candidates at 5 even if more findings exist
- User corrections always rank above tool failures
- The multi-select in Step 1 IS the approval — never re-confirm per action
- journal.md checkpoint is mandatory regardless of what else runs
- decisions.md is for load-bearing structural decisions only, not operational notes
- If the session used no skills, only offer memory and CLAUDE.md candidates
- Keep the entire interaction to two moments: one question call, then silent execution

---

## Tools

- AskUserQuestion
- Read
- Edit
- Write
- Bash (file discovery for multi-session mode only)
