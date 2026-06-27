---
name: level-up
description: Weekly growth session for your AIOS. Trigger on: "let's level up", "what should I improve", "find me leverage this week", "what should I automate next", "my AIOS feels stale", "something isn't working right", "update my context", "I want to build something", "help me get more out of my system", or as a regular end-of-week ritual. Two modes — Add (find and build one new capability) and Improve (fix or refresh what exists). One session = one concrete output.
---

## What this skill does

A structured weekly session to either add a new capability to your AIOS or improve something that already exists. Every session ends with one concrete output — a saved prompt template, a refined skill, a refreshed context file, or a new tool connection.

This is how the system gets better over time. Running it consistently, even for 20 minutes a week, compounds faster than any single optimization.

**Folders this skill uses:** `templates/` at the root level for reusable prompts (created on first save if it does not exist). Domain-specific prompt templates can also live in `[bucket]/templates/` if you prefer to keep them organized by domain — either works.

---

## What `/level-up` is NOT

- **Not `/audit`.** Audit is a health check — "is the system built right?" Level-up is an action session — "what's the next thing to fix or build?" Run `/audit` if something feels fundamentally broken before running level-up.
- **Not a planning session.** One run = one output. No multi-candidate parallel scoping.
- **Not only for adding things.** Mode B exists specifically for improving what's already there.

---

## When to run

- **Week 2 after onboarding:** First run, after at least one tool is connected and one session has been logged.
- **Weekly after that:** End of week works well — review what happened, build something for next week.
- **Any time something isn't working right:** A skill triggering wrong, context that feels outdated, a task that keeps taking longer than it should.

---

## What this skill reads

From the current directory first; root as fallback:

- `CLAUDE.md` — system overview and bucket structure
- `connections.md` — what tools are reachable and how
- `decisions.md` — what has been decided and built
- `context/priorities.md` and, for each active bucket, any priorities file in `[bucket]/context/` — what matters this quarter (if present)
- Most recent `audits/audit-{date}.md` — gap context from last audit (if saved)
- `.agents/skills/*/SKILL.md` frontmatter — installed capabilities
- `learnings/` folder — recurring friction signals

---

## Mode selection

Open every session by asking:

> "Two modes today:
>
> **Mode A — Add something new.** You've spotted a friction or opportunity and want to build a prompt, skill, or workflow that handles it. Good for: repetitive tasks, things you do the same way each time, manual work that should be automatic.
>
> **Mode B — Improve what exists.** Something isn't quite right, your context has gone stale, or a connection isn't being used. Good for: skills that aren't triggering right, priorities that are months out of date, tools you set up but never use.
>
> Which fits where you are today — or are you unsure?"

If unsure: "Have you run an audit recently? Your top 3 gaps usually point to the right mode. If not, Mode A is a good default."

---

## MODE A — Add

*Find one friction, scope one solution, build one thing.*

### Phase 1 — Find

Establish scope and surface candidates. Ask these conversationally, one at a time:

1. "Which area of your work are we focused on today?" *(Establish the bucket. This scopes everything that follows.)*

2. "Walk me through your week. What did you do more than twice that felt like repetitive work?"

3. "Was there anything where you thought 'a prepared version of this would save me time every time I face it'?"

4. "If your workload doubled tomorrow, what would break first in this domain?"

5. "What's one thing that, if it just happened automatically, would unlock the most momentum?"

**Optional web search after question 5:** Search `"Claude Code skill [user's domain] productivity [current year]"`. Surface any community-built skills or workflows relevant to their situation. If a `research` skill exists in your library, use it for a deeper look. Keep findings to one short paragraph — don't derail the session.

**Output of Phase 1:** 1–3 candidate opportunities. For each, one sentence on why it's leverage. Then: "Pick one to scope."

---

### Phase 2 — Scope

*Understand the problem clearly before building anything.*

**First: should we even build this?**

Before anything else, ask: "What happens if we just stop doing this task altogether?"

If the answer is "nothing important breaks" — do not automate waste. This is a win. Log it to `decisions.md` as: "Decided to eliminate [task] — not worth automating or doing." Exit cheerfully.

**Map the work** using five questions:

1. What kicks it off? *(The trigger — what situation or event starts this task?)*
2. What information does it need to do the job?
3. What does it do with that information?
4. Where does the result go?
5. Where could it go wrong, or where does it need a human decision?

If the user cannot answer these clearly: "If you can't explain this clearly to a person, it is too vague to build right now. Sketch it out first — even rough notes — then come back."

**Choose an autonomy level:**

The Autonomy Ladder — start at the lowest level that solves the problem. Move up only after you have seen it work at the lower level.

| Level | Name | What it means in practice |
|---|---|---|
| 1 | Prompted | You run it manually when you need it. Claude helps on request. |
| 2 | Drafted | Claude prepares a draft. You review, edit, and decide. |
| 3 | Supervised | Claude runs the task. You check the output periodically. |
| 4 | Scheduled | Claude handles it on a trigger or schedule. You see the result. |
| 5 | Autonomous | Claude monitors and acts. You are notified only when something unusual happens. |

Push back on Level 4–5 unless the user has validated lower levels first. "Autonomous workflows built on unclear processes produce confident wrong answers."

**Define success:** "Name one thing that will measurably improve — time saved per week, fewer back-and-forth messages, less chance of something slipping through. If you cannot name it, the scope is not clear enough yet."

Write the scoped spec to `decisions.md` as a dated entry: what it does, the trigger, the autonomy level chosen, and the success metric.

---

### Phase 3 — Build

"How do you want to ship this?"

Four options, ordered from simplest to most complex. **Default = the simplest option that solves the problem.** The user has to explicitly choose more complexity.

---

**Option 1 — Saved prompt template**

A reusable instruction you call up when you need it. Zero infrastructure. Highest manual involvement, but faster than starting from scratch every time.

*Best for:* Tasks you do weekly or less often. Anything that needs your judgment each time. A strong first version of any automation — run it manually before you automate it.

*Example:* A template that drafts a weekly status update in your voice, pulling from your priorities. You paste it, fill in this week's specifics, send.

Build it inline. Save to `templates/` at root (or `[bucket]/templates/` if it belongs to one specific domain) with a clear filename.

---

**Option 2 — Skill**

Claude follows a defined process when you trigger it with a specific phrase or command. Runs consistently without you re-explaining the steps each time.

*Best for:* Multi-step tasks with a clear start and end. Processes that run the same way each time. Work you do at least weekly.

*Example:* A skill that opens with the right questions for your weekly planning session and ends with a prioritized task list in your format.

Route to `skill-creator` if available in your skill library.

---

**Option 3 — Connected workflow**

Claude pulls from one of your connected tools — calendar, task manager, email — to do the work with live data from your actual systems.

*Best for:* Anything that needs current information from your real tools, not just what you tell it in the chat.

*First check:* Do you have the right connection for this? Look at `connections.md`. If the tool is not connected:
1. Search the Printing Press library first: `npx -y @mvanhorn/printing-press-library search [tool name]`. If a CLI exists, Claude Code can install it in minutes — Go installation included.
2. If not in the library, consider an MCP connection as the fallback.

---

**Option 4 — Autonomous agent**

Claude monitors something and acts without you asking. Work happens between your active sessions.

*Best for:* High-frequency, rule-based tasks where constant human review is not practical. This is the top of the Autonomy Ladder.

*Only go here after validating at Option 1 or 2 first.* "Autonomous work built on unvalidated processes creates confident errors."

---

**Every built artifact includes this line in its frontmatter:**

```yaml
autonomy-level: 1  # Start here. Move up only after validating at this level.
```

This is not optional — it is a reminder that every new capability should be proven manually before it runs on its own.

---

**Output contract for every Mode A session:**

1. One `decisions.md` entry — what was scoped, the autonomy level, the success metric
2. One concrete output — prompt template, skill file, or connection plan
3. One closing line — what was built and what it is expected to do

---

## MODE B — Improve

*Fix or refresh something that already exists.*

Mode B has three types. Open by identifying which fits:

> "Three types of improvement — which one is this?
>
> **1. Something isn't working right.** A skill triggers at the wrong time, produces weak output, or does the wrong thing. You want to fix it, not replace it.
>
> **2. Your context has gone stale.** Your priorities have changed, people have shifted roles, or your system is working off an outdated picture of your life and work.
>
> **3. A connection isn't pulling its weight.** A tool you set up isn't being used, is broken, or you want to connect something new.
>
> Which one?"

---

### Mode B-1 — Skill refinement

Ask: "Which skill, and what's the problem — it fires at the wrong time, the output isn't right, or it's doing the wrong thing altogether?"

**Diagnose the issue:**

- **Wrong trigger** (fires when it shouldn't, or doesn't fire when you need it): the problem is in the skill's description frontmatter. It needs more specific trigger phrases, better exclusion conditions, or clearer context.
- **Wrong output** (produces the right type of thing but in the wrong format, length, or style): the problem is in the execution section. The instructions need to be tightened.
- **Wrong scope** (does too much, too little, or something adjacent to what you need): the scope needs a focused conversation first, then a rewrite.

**Route:**

If `skill-refine` is available in `.agents/skills/skill-refine/`, route there.

Otherwise, use `skill-creator` with this framing:
> "We are refining an existing skill, not building a new one. The specific problem is [trigger / output / scope]. Here is the current skill. Focus only on fixing [the specific issue] without changing what is already working."

Log the refinement to `decisions.md` with a note on what changed and why.

---

### Mode B-2 — Context freshness

Walk through each area that may have drifted. Check modification dates on context files before asking — only prompt for areas that are actually stale (>90 days) or absent.

**Priorities:** If a priorities file exists in any bucket's `context/` folder and has not been updated in 90+ days: "Your priorities were last updated [X days ago]. What are your 2–4 biggest outcomes for the next 90 days?" If no priorities file exists, offer to create one. Update or create `priorities.md` in each relevant bucket's `context/` folder.

**People:** If a people file exists in any bucket's `context/` folder: "Anyone new who matters in your domains? Anyone who has moved on or changed roles?" Update accordingly. If it does not exist, offer to create one.

**Operating manual:** "Has anything fundamental changed — a new bucket, a major role shift, a tool that is now central to your daily work?" Update root `CLAUDE.md` if so.

**Persona:** "Is the communication style in `SOUL.md` still accurate? Does it still reflect how you want to sound?" Review and update if needed.

Back up originals to `archives/context-refresh-{date}/` before overwriting any file that has changed significantly.

---

### Mode B-3 — Connection health

Pull `connections.md`. Walk through connected tools:

- When was each last actively used in a session?
- Is the connection still working?
- Is it delivering value, or was it set up and then forgotten?

For unused connections: "Do you want to learn how to use it, or remove it from your setup?"

For missing tools or broken connections: search the Printing Press library first (`npx -y @mvanhorn/printing-press-library search [tool]`), then MCP as fallback.

**Optional web search:** `"new [user's tools] Claude Code integration [current year]"` — surface any new connection options worth considering. Use `research` skill if available.

---

**Output contract for every Mode B session:**

1. One `decisions.md` entry — what was reviewed and what changed
2. One concrete output — updated skill, refreshed context file, or connection change
3. One closing line — what improved and what to watch for next

---

## Critical implementation rules

1. **One session = one output.** No multi-candidate parallel scoping in Mode A.
2. **Mode selection always happens first.** Even if the user arrives with a formed idea, confirm Mode A or B before proceeding.
3. **Eliminate before building.** In Mode A Phase 2, always ask "what if we stopped doing this?" before scoping an automation.
4. **Default to the simplest build option.** The user has to choose more complexity, not less.
5. **Autonomy level ships in every artifact.** `autonomy-level: 1` in frontmatter is not optional.
6. **Never modify existing files without confirmation.** In Mode B-2, show what will change before writing.
7. **PP CLI before MCP.** For any new connection, check the Printing Press library first. Note which mechanism is used when logging to `decisions.md`.
8. **Bucket-aware reads.** Read context from the active domain first; fall back to root. Mode A is always scoped to one bucket.
