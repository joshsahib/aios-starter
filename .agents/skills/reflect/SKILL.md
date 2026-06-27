---
name: reflect
description: Quarterly strategic review of your AIOS. Trigger on: "reflect on my system", "quarterly review", "how has my AIOS improved", "where is my system headed", "is my AIOS getting better", "strategic review", "look at my progress", or when it has been 60+ days since the last reflect. Synthesizes audit history, learnings, and decisions into a strategic view of system maturity. Read-only except for an optional saved report.
---

## What this skill does

Where `/audit` is a periodic health check and `/level-up` is a weekly action session, `/reflect` is the long view.

It reads across time — audit history, decisions made, learnings accumulated, session patterns — and synthesizes: is your AIOS actually getting better? Are the same problems recurring? Where are you on the path toward a system that works without being pushed, and what is in the way?

Every reflect session ends with one strategic recommendation for the quarter ahead. Not a weekly task — a quarter-level commitment.

**Run monthly at minimum. Quarterly for a full strategic review.**

**Folders this skill uses:** `reflects/` at the root level (alongside `learnings/` and `audits/`). Created automatically on first save if it does not exist. Also reads from `audits/` — save your audit reports to get the most out of reflect.

---

## What this skill reads

Look for these files — treat each as optional and note which are absent rather than erroring:

- `audits/` folder at root — saved audit reports (the score history)
- `reflects/` folder at root — previous reflect reports (the strategic history)
- `decisions.md` — what has been decided and built
- `learnings/` folder — all behavioral corrections saved across sessions
- `journal.md` — recent session activity
- Root `CLAUDE.md` — system overview and confirmed bucket names
- For each active bucket, look in `[bucket]/context/` for priorities, domain description, and people files — commonly named `priorities.md`, `about-[bucket].md`, and `people.md`, but names may vary
- `.agents/skills/*/SKILL.md` frontmatter — installed capabilities

Missing files are themselves signals worth noting in the report.

---

## The AIOS Maturity Ladder

Understanding where a system sits requires a shared vocabulary. These stages describe real behavioral thresholds, not scores.

**Stage 0 — Foundation**
The system exists but is not working yet. Context is minimal or placeholder. You find yourself re-explaining who you are and what matters at the start of most sessions. No recurring triggers. Skills are installed but rarely used.
*Score signal: Cadence and Continuity both below 10. Journal sparse or empty.*

**Stage 1 — Built**
The system is connected to real tools and actively used. Sessions are productive. You are doing most of the prompting, but the system knows who you are and what matters. Some skills are running for defined tasks.
*Score signal: Audit total 40–69. Level-up run at least once. At least 2 Tier-1 domains reachable.*

**Stage 2 — Compounding**
Automations are running. Context stays reasonably fresh. Learnings are accumulating and shaping behavior between sessions. The system is getting smarter. You are starting to rely on it rather than manage it.
*Score signal: Audit total 70–89. Continuity above 14. Recurring triggers exist. Learnings folder has 5+ entries.*

**Stage 3 — Autonomous**
Scheduled work happens without prompting. Context is maintained proactively. Skills are well-tuned and rarely misfire. The system is an active participant in your work, not just a tool you pick up when you remember to.
*Score signal: Audit total 90+. Hooks or scheduled jobs running. Multiple PP CLIs connected. At least one skill has been refined since initial install.*

**Beyond Stage 3 — The horizon**
More advanced systems begin to involve multiple AI models cross-checking each other's work, local LLMs handling routine tasks for token efficiency, and work being delegated to autonomous agents that run between your active sessions. These are design goals, not Day 1 requirements — but knowing they exist helps you see the direction of travel and design toward it from the start.

---

## Execution

### Step 1: Assess what data is available

Before synthesizing, check what you have to work with:

- How many saved audit reports are in `audits/`? What date range do they cover?
- Are there previous reflect reports in `reflects/`?
- How many entries are in `learnings/`?
- How many entries in `decisions.md`?
- When was `journal.md` last updated?

Note any missing inputs upfront:

- **No audit history:** "I cannot show score trends yet — no audits have been saved. Establish the baseline by running `/audit` and saving the report, then reflect next month." Still proceed with what is available.
- **First reflect ever:** Say so explicitly. "This is your first reflect — we are establishing a baseline, not measuring change."
- **Sparse decisions or learnings:** Note it as a signal — the system is not capturing enough of its own activity to learn from.

---

### Step 2: Score trend analysis

**If 2+ audit reports exist:**
- List scores chronologically: `58 → 67 → 74 → 79`
- Identify which C improved most across that span
- Identify which C is stuck or declining
- Note any criterion that has never scored above "Thin" across multiple audits — this is a structural gap, not a one-time miss

**If 1 audit exists:**
- Show current state vs. the starting point
- Note what has changed since

**If no audit history:**
- Skip this section. Recommend running `/audit` and saving the report as the first action after this reflect.

---

### Step 3: Pattern analysis

**From `learnings/`:**

Read all behavioral correction entries. Look for recurring themes:
- The same type of correction appearing 3+ times → a skill needs to be built or refined to handle this permanently, not just noted
- Corrections about voice or tone → `SOUL.md` needs updating
- Corrections that were entered once and do not recur → fixed permanently (flag these as wins)

**From `decisions.md`:**

Look for:
- Decisions scoped but never built → stalled work worth revisiting or officially closing
- Things built but never referenced again in subsequent decisions → possibly abandoned or not working as intended
- A pattern of the same type of decision (same domain, same kind of problem) → this bucket needs more capability investment

**From `journal.md`:**

- Has session frequency been consistent or dropped off?
- Are sessions concentrated in one bucket and thin in others?

---

### Step 4: Maturity assessment

Using the stage definitions above, identify which stage the system is currently in and what specifically would unlock the next one.

Frame this as forward-looking and encouraging, not as a judgment. The goal is to give the person a clear picture of where they are and a concrete sense of what comes next.

Format:

> "Your system is at **Stage [N] — [name]**. [One sentence on what's working that earned this stage.]
>
> **What would unlock Stage [N+1]:** [The specific gap — honest and concrete. E.g., 'Adding one recurring trigger would move the system from something you invoke manually to something that runs on its own.' Or: 'Your learnings folder has 2 entries — building that up to 5+ over the next few months will start giving the system real behavioral memory.']"

If the system has been at the same stage across multiple reflects, frame it as an opportunity rather than a failure: "You've been building a solid Stage 1 foundation for several quarters — the context and connections are there. The data consistently points to [X] as the shift that would move things to Stage 2." Then recommend that shift as the quarterly focus.

Progress is rarely linear. A stage held steadily while other areas of life were demanding is not failure — it is resource allocation. The maturity ladder is a direction, not a deadline.

---

### Step 5: Strategic synthesis

Produce three outputs:

**Top wins since last reflect** (1–3 bullets, specific):
Not morale-building generalities — concrete improvements. "Connections score moved from 8 to 16 after adding the Linear CLI. Sessions in the startup bucket are now 3x more frequent than six months ago."

**Persistent gaps** (1–3 bullets):
Problems that have appeared in multiple audits or learnings without resolution. For each: name the gap, note how many times it has surfaced, and recommend a structural change rather than another one-time fix.

**One strategic recommendation for the quarter:**
The single most important investment in the system this quarter. This is different from a level-up recommendation. It is a quarter-level commitment:
- Building a whole new domain connection for a bucket that is disconnected
- Establishing a daily cadence ritual that does not yet exist
- Investing in skill refinement for a capability that is being used but is underperforming
- Deliberately moving to a higher autonomy level on a workflow that is already validated

Make it specific. Not "improve context" but "update priorities for all three buckets and schedule a monthly reminder to do it again."

---

### Step 6: Optional web search

Search `"AI productivity system best practices [current year]"` and `"Claude Code autonomous workflow [current year]"`. Surface any new patterns or capabilities that were not available at the time of the last reflect. Use the `research` skill if it exists in your library for a deeper scan. Keep findings to one short paragraph — signal check, not deep dive.

---

### Step 7: Output the report

```
# AIOS Reflect — {date}

**Stage: {N} — {stage name}**
{One-sentence description of what that stage means for this system.}

---

## Score trajectory

{date}: {score}/100  Stage {N}
{date}: {score}/100  Stage {N}
{date}: {score}/100  Stage {N}
→ {Trend summary. E.g., "Connections has stalled at 10–12 across four audits.
   Every other C is improving."}

(No audit history — run /audit and save the report to start tracking.)

---

## What improved
- {Specific win — cite scores or capabilities where possible}
- {Specific win}

## What's stuck
- {Persistent gap} — appeared in {N} audits without resolution
- {Persistent gap}

## Patterns in learnings
{1–2 sentences. E.g., "Your learnings folder has 8 entries, 5 of which are about
voice and tone. This suggests SOUL.md needs a meaningful update, not just a review."}

---

## Maturity assessment

**You are at Stage {N} — {stage name}.**
{One sentence on what is working.}

**What would unlock Stage {N+1}:** {Specific, actionable gap — what needs to exist or happen that doesn't yet.}

---

## This quarter's strategic recommendation

{One specific, actionable investment. Not a weekly task — a quarter-level commitment.}

---

## What to watch for next reflect
- {1–2 things to look for at the next review — specific leading indicators}

---
{Emerging practices — short paragraph if web search ran}
```

---

### Step 8: Offer to save the report

"Save this to `reflects/reflect-{date}.md`? Future reflects read this history to show you the trajectory over time."

If yes: write the file, creating `reflects/` if it does not exist. **This is the only writable side effect.**

---

## Notes

- **Read-only by default.** Never modify context files, skills, or system files during a reflect run.
- **Honest over generous.** If the system has not improved, say so and say why. A flat score trend is useful information.
- **First reflect is a baseline, not a report card.** If there is no history, say so and set expectations for what to look for at the next one.
- **Do not recommend building things that do not exist** in the user's skill library. Point at `/level-up` to scope new builds.
- **Strategic, not tactical.** The single recommendation should be a quarter-level investment, not something `/level-up` Mode A would normally surface.
- **Dynamic bucket detection.** Read bucket names from root `CLAUDE.md` — never hardcode names.
- **Forward compatibility note.** As AI systems mature, reflect sessions may eventually involve multiple models cross-checking each other's assessments. The scoring and pattern analysis in this skill is designed to be model-agnostic — no assumptions about which model is running the review.
