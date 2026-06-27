---
name: audit
description: Monthly AIOS health check. Trigger on: "audit my setup", "score my AIOS", "is my system working", "check my setup", "how healthy is my AIOS", "find gaps in my system", "audit my AIOS", or any request to evaluate overall system quality. Scores the Five Cs out of 100. Produces a scoreboard with top-3 leverage-weighted gaps and concrete next steps. Read-only except for an optional saved report.
---

## What this skill does

Runs the **Five Cs Audit** on your AIOS. Reads — never writes — your operating manual, context files, skills, connections, and session history. Scores each of the Five Cs out of 20. Surfaces strengths, top 3 leverage-weighted gaps, and concrete next steps.

**Scope: structural and behavioral.** The audit answers two questions: "Is the AIOS built right?" and "Is it being used consistently?" It does not plan new capabilities — that belongs to `/level-up`. If a gap surfaces that requires building something, the audit points you there.

First run is your baseline. Save every audit to `audits/` and watch the score climb.

**Folder this skill uses:** `audits/` at the root level (alongside `learnings/` and `archives/`). Created automatically on first save if it does not exist.

## The AIOS Maturity Stages

| Score | Stage | What it means |
|---|---|---|
| 0–39 | Stage 0 — Foundation | The system exists but isn't working yet |
| 40–69 | Stage 1 — Built | Connected and running, mostly manual |
| 70–89 | Stage 2 — Compounding | Automations running, patterns accumulating |
| 90–100 | Stage 3 — Autonomous | Work happening without prompting |

Most systems land at Stage 1–2 for the first several months. That is normal and expected.

## The Five Cs (20 points each = 100 total)

| C | The question it answers |
|---|---|
| **Context** | Does the system know you — your identity, domains, voice, priorities, and people? |
| **Connections** | Can it reach the tools and data sources that matter to your actual work? |
| **Capabilities** | Does it know how to do useful work across your domains? |
| **Cadence** | Is it running consistently without you having to push it every time? |
| **Continuity** | Is it learning, improving, and staying current over time? |

---

## Execution

### Step 1: Discover the project shape

**Always read bucket names from the root `CLAUDE.md` bucket table.** Never assume `personal-family/`, `day-job/`, or `business-hobby/`. The bucket structure is defined by the user during onboarding and may use any names.

Look for these files — treat each as optional and note which are missing rather than erroring:

**System files:**
- `CLAUDE.md` (root) — operating manual
- `SOUL.md` — global persona
- `journal.md` — session diary
- `learnings/` folder — behavioral corrections
- `connections.md` — tool registry and primary source of truth for what is connected
- `decisions.md` — decision log
- `audits/` folder — previous audit reports
- `reflects/` folder — previous reflect reports

**Context files (if present):**
- `context/about-me.md` and `context/voice.md`
- For each active bucket, look inside `[bucket]/context/` for files capturing priorities, domain description, and key people. These are commonly named `priorities.md`, `about-[bucket].md`, and `people.md` — look for files with these names or similar intent. Not all will exist, especially in early-stage setups.

**Capability files:**
- `.agents/skills/*/SKILL.md` — count and read frontmatter only for speed
- `.agents/agents/*.md` — count

**Connection detection:**

Start with `connections.md` — it is the canonical record of what is connected and how. If a tool is documented there as active, count it as reachable regardless of mechanism. Then verify active connections using the checks below.

Mechanism tiers (prefer earlier tiers — they are more token-efficient):

1. **Printing Press CLIs** (highest efficiency): for any tool listed in `connections.md`, run `which [tool]-pp-cli` or `which [tool]-goat` to confirm the binary is installed. A PP CLI uses ~35x fewer tokens than an MCP and is purpose-built for agent use. Flag PP CLI connections as optimal in the report.
2. **MCP servers**: `.mcp.json`, `.claude/settings.json` (mcpServers key), `.claude/settings.local.json`
3. **Scripts**: `scripts/*.py`, `scripts/*.js`, `scripts/*.ts` documented in `CLAUDE.md` or `connections.md`
4. **Export pipelines**: `data/`, `imports/`, `exports/` folders with documented refresh process

Any of these mechanisms makes a domain "reachable." When `connections.md` and the file-system checks conflict, note the discrepancy rather than silently choosing one.

---

### Step 2: Score each C

#### Context — 20 points

*Does the system know you well enough to do useful work without being reminded?*

| Criterion | Pts | How to detect |
|---|---|---|
| Root `CLAUDE.md` is substantive (>200 words, not mostly placeholder text) | 3 | Read and count words |
| `SOUL.md` exists and is filled (≤2 bracketed placeholders remaining) | 3 | Read, count `[bracketed]` items |
| Identity, domains, and voice are captured | 3 | `CLAUDE.md` or `context/` describes who the user is, their active buckets, and how they communicate |
| Bucket-level context exists for at least one bucket | 3 | At least one `[bucket]/context/` contains `priorities.md` and `about-[bucket].md` with real content (not placeholders) |
| Key people are captured | 2 | At least one `[bucket]/context/people.md` exists with real entries |
| Priorities are fresh | 3 | At least one priorities file was modified within the last 90 days. Stale priorities = the system is navigating by an old map. |
| Decisions and references exist | 3 | `decisions.md` has ≥1 entry; `references/` or `docs/` or `sops/` has ≥1 file |

---

#### Connections — 20 points

*Can the system actually reach the data and tools that make up your working life?*

**The 7 Tier-1 Domains** — these are the categories that, if unreachable, mean the AIOS is working blind:

| # | Domain | Examples |
|---|---|---|
| 1 | Revenue / Financials | Banking, accounting, invoicing, payments |
| 2 | Customer / Relationships | CRM, contacts, deal pipeline, relationship tracking |
| 3 | Calendar | Scheduling, availability, upcoming commitments |
| 4 | Communication | Email, messaging apps, team chat |
| 5 | Project / Task tracking | Task managers, issue trackers, project boards |
| 6 | Meeting intelligence | Recordings, transcripts, meeting notes |
| 7 | Knowledge / Files | Documents, notes, knowledge bases, file storage |

A domain is "reachable" if the system can pull relevant data from it via any mechanism. PP CLI is preferred (flag as optimal), MCP and scripts both count.

| Criterion | Pts | How to detect |
|---|---|---|
| Tier-1 domain coverage | 10 | 1.4 pts per reachable domain. Round to nearest 0.5. Cap at 10. |
| Connections documented in `connections.md` | 4 | 0 if missing; 1 sparse (<3 tools documented); 2 partial; 3 most tools; 4 comprehensive with mechanism and status noted |
| At least one connection can write (send, post, create — not just read) | 3 | Verify at least one integration supports write operations. A read-only AIOS is a viewer, not an operating system. |
| No expired or broken connections | 3 | −1 per connection flagged as `needs-auth`, `expired`, or known broken. Floor 0. |

**When flagging unreachable Tier-1 domains in the gap report:** always suggest checking the Printing Press library first before recommending MCP setup. The library has 50+ pre-built CLIs and Claude Code can handle installation. Search with: `npx -y @mvanhorn/printing-press-library search [tool name]`

---

#### Capabilities — 20 points

*Does the system know how to do useful, repeatable work for this specific person?*

| Criterion | Pts | How to detect |
|---|---|---|
| 3 or more skills installed | 6 | Count `SKILL.md` files in `.agents/skills/` or `.claude/skills/` |
| Skills are relevant to the user's actual domains | 4 | Compare skill names and descriptions against the bucket structure and domains in `CLAUDE.md`. Domain-specific skills score higher than generic utilities. |
| At least one skill built or meaningfully customized after initial install | 5 | Compare skill file modification dates against `aios-intake.md` creation date. Give benefit of the doubt on Dropbox-synced systems where timestamps may not be reliable — look for evidence of customization in skill content instead. |
| Skill trigger descriptions are specific (Claude will actually use them) | 3 | Spot-check 2–3 skill frontmatter descriptions. Vague ("use for writing tasks") scores 0. Specific (lists phrases, contexts, trigger conditions) scores full. |
| At least one agent defined | 2 | Count `.agents/agents/*.md` ≥1. Low weight — agents are advanced and optional. Strong skill usage without agents is equally valid. |

**Undocumented skill check:** Scan all directories in `.agents/skills/`. For any directory containing a `SKILL.md` not listed in the skill inventory in `HANDBOOK.md`, add a flag to the Capabilities section of the report: "Undocumented skill found: `[skill-name]` — add it to `HANDBOOK.md`." This does not affect the Capabilities score but surfaces a maintenance gap.

---

#### Cadence — 20 points

*Is the system running consistently, or does it only work when the user consciously decides to use it?*

| Criterion | Pts | How to detect |
|---|---|---|
| `journal.md` has entries within the last 14 days | 6 | Check most recent dated entry. Active journal = the system is being used in real sessions. |
| At least one recurring trigger or scheduled automation exists | 5 | `.claude/settings.json` hooks key, OR a skill named `morning-*`, `daily-*`, `weekly-*`, or `standup` |
| `/level-up` has been run at least once | 4 | `decisions.md` has a level-up entry, OR `audits/` has ≥1 previously saved report |
| `threads/` folder has content (active topic capture is happening) | 3 | Any `[bucket]/threads/` or root `threads/` has ≥1 non-empty file |
| Reusable templates or prompt templates exist | 2 | `templates/`, `[bucket]/templates/`, or `.claude/templates/` has ≥1 file |

---

#### Continuity — 20 points

*Is the system learning from experience and improving over time — not just running the same way it started?*

| Criterion | Pts | How to detect |
|---|---|---|
| `learnings/` has behavioral corrections | 5 | 0 files = 0 pts; 1–3 files = 2 pts; 4–7 files = 4 pts; 8+ files = 5 pts |
| `decisions.md` is actively used (entry within the last 60 days) | 4 | Check most recent entry date |
| Score is trending up vs. previous audit | 4 | Read `audits/` for most recent saved report, parse total score, compare. If no previous audit exists: award 2 pts (baseline run, not a penalty) |
| Context files are fresh across all active buckets | 4 | For each active bucket, look for a priorities file in `[bucket]/context/`. If found, check its modification date. All fresh (<90 days) = 4; mixed = 2; all stale (>90 days) or none found = 0 |
| Previous audit is saved and accessible | 3 | `audits/` has ≥1 saved report with a legible score |

---

### Step 3: Identify top 3 gaps by leverage

For each criterion that lost points, calculate: **leverage score = points lost × impact multiplier**

**Impact multipliers:**

| Gap | Multiplier | Why |
|---|---|---|
| 0 Tier-1 domains reachable | 4× | The AIOS is completely blind to the user's actual work |
| ≤2 Tier-1 domains reachable | 3× | Still fundamentally disconnected |
| `CLAUDE.md` missing or thin | 3× | The foundation of every session |
| `SOUL.md` all placeholders | 3× | Every session starts cold without a persona |
| Priorities stale or missing | 3× | The system is navigating by an outdated map |
| Score trending down vs. previous audit | 2× | Regression needs diagnosis before it compounds |
| 0 skills beyond initial install | 2× | No capability growth has happened |
| No recurring trigger | 2× | No Cadence = the AIOS only works when manually invoked |
| All connections read-only | 2× | A viewer, not an operating system |
| Bucket context empty | 2× | The system has no idea what matters in any domain |
| No `learnings/` entries | 2× | The system is not getting smarter between sessions |
| No `journal.md` entries in 14 days | 1.5× | No session continuity |
| No decisions log | 1.5× | Nothing is being remembered between level-up runs |
| All others | 1× | Standard leverage |

Sort by leverage score descending. Take the top 3. For each gap, write one concrete next step:

- **No/thin `CLAUDE.md`** → "Run `/onboard` to populate your operating manual and context files."
- **`SOUL.md` unfilled** → "Fill in the bracketed sections — especially communication preferences and tone. This file loads every session."
- **Tier-1 domain unreachable** → "Search the Printing Press library first: `npx -y @mvanhorn/printing-press-library search [tool]`. If found, Claude Code can install it. If not, run `/level-up` to scope a connection."
- **Priorities stale** → "Run `/level-up` → Mode B → Context freshness to update your priorities files."
- **No user-built skill** → "Run `/level-up` → Mode A to surface and build your highest-leverage capability."
- **No recurring trigger** → "Run `/level-up` and in the Build phase, choose 'scheduled trigger' as the output format."
- **No `learnings/` entries** → "After your next session, run `/retro` and save at least one behavioral correction to `learnings/`."
- **No recent journal entries** → "Start logging sessions — even one line per session. Run `/retro` at the end of each session."
- **Score trending down** → "Run `/reflect` if it has been ≥30 days since the last audit. Or run `/level-up` Mode B to diagnose the specific regression."
- **All connections read-only** → "Run `/level-up` and look for one tool you could write to — sending a summary, creating a task, updating a record."

---

### Step 4: Optional web search

Search `"AIOS productivity best practices [current year]"` and `"Claude Code workflow knowledge worker [current year]"`. Surface any emerging patterns not yet reflected in this system. If a `research` skill exists in your skill library, use it for a deeper scan. Keep findings to a short paragraph — this is a signal check, not a deep dive.

---

### Step 5: Output the report

Print directly in chat:

```
# AIOS Audit — {date}
**Score: {total}/100** — Stage {N}: {stage name}

## Scoreboard

Context       {████░░░░░░}  {n}/20  {label}
Connections   {████░░░░░░}  {n}/20  {label}
Capabilities  {████░░░░░░}  {n}/20  {label}
Cadence       {████░░░░░░}  {n}/20  {label}
Continuity    {████░░░░░░}  {n}/20  {label}

(Each █ = 2 pts. Labels: Strong ≥17, Solid 13–16, Thin 8–12, Missing <8)

## Bucket snapshot
[bucket-name]   Context ✓/✗   People ✓/✗   Priorities: fresh/stale/missing
[repeat for each active bucket]

## Strengths
- {1–3 bullets from highest-scoring criteria — be specific}

## Top 3 Gaps (ranked by leverage)

1. **{gap name}** (−{pts} lost × {N}x leverage)
   → {one concrete next step}

2. **{gap name}** (−{pts} lost × {N}x leverage)
   → {one concrete next step}

3. **{gap name}** (−{pts} lost × {N}x leverage)
   → {one concrete next step}

## Focus: {single most important action, one sentence}

---
Structural and behavioral gaps only.
For capability gaps (what your AIOS could do that it can't yet), run /level-up.
For strategic trend analysis across multiple audits, run /reflect.
{Emerging practices — short paragraph if web search ran}
```

---

### Step 6: Offer to save the report

After printing, ask: "Save this audit to `audits/audit-{date}.md`? Saving lets `/reflect` track your score trend over time and show whether the system is actually improving."

If yes: write the file, creating the `audits/` folder if it does not exist. **This is the only writable side effect of this skill.**

---

## Notes

- **Read-only by default.** Never modify `CLAUDE.md`, `SOUL.md`, `learnings/`, skills, or any project files during an audit run.
- **Dynamic bucket detection always.** Read bucket names from root `CLAUDE.md`. Never hardcode names.
- **Be honest, not generous.** A 90/100 is a serious achievement. Most active systems run 45–72. Inflated scores are useless.
- **Be flexible on file names.** Don't penalize for non-canonical names if the intent is clearly captured elsewhere.
- **Speed matters.** Target under 60 seconds. Read targeted files; check skill count by counting folders, not reading full contents.
- **Do not suggest skills that do not exist** in the user's library.
- **PP CLI is preferred.** When flagging connection gaps, always check the Printing Press library before recommending MCP. Note which connections are PP CLI vs. MCP in the report — PP CLI connections are more token-efficient and worth highlighting.
