# AIOS: Personal AI Operating System

A structured starter kit for building a personal AI Operating System on top of your chosen AI harness. Designed for knowledge workers running multiple life domains in parallel: professionals, founders, creators, and anyone managing a complex life who wants their AI to know who they are, what matters, and how they work across all of it.

---

## The goal

> **"While you're not at your desk, your AIOS observes one real-world event and produces an output that's faster and more accurate than what you'd produce yourself."**

Every design decision in this kit rolls up to that test. If a layer, skill, or template does not contribute to it, it does not ship.

---

## How you'll know it's working

Three felt success indicators tell you the AIOS is actually changing how you work.

**1. Team-reaches-out**

> *"A teammate messages you with a question. You realize your AIOS would answer it better, faster, and with exact sources, even if you were awake and free. So you ask your AIOS too. That's the moment you stop being a bottleneck for your own knowledge."*

**2. Mental load drops**

> *"You stop rehearsing facts, decisions, and context in your head. You stop trying to remember what was decided last quarter, what someone told you in that meeting, what the right answer is to a question you have answered before. You trust the retrieval. The AIOS holds the truth; you hold the questions."*

**3. The default surface shifts**

> *"You stop opening new tabs. When something lands, your first move is to ask the AIOS, not to launch six things. Thought work consolidates. The compound effect is quiet and steady."*

**The bigger picture.** As the system matures toward the agent layer, it begins handling recurring tasks in the background, surfacing the right information at the right moment, drafting things you would otherwise write from scratch, and keeping track of what matters so you do not have to. The result is not just faster output. It is a reduction in the mental overhead of managing a complex life, and more time and attention for the things that actually require you.

For people who already use a personal knowledge management or second brain system, an AIOS integrates naturally as the action layer on top of that captured knowledge. The two are complementary: one stores, the other acts.

**From personal to organizational.** Once these indicators show up for one person, the same architecture powers everything else: custom workflows on the data already collected, automations on the connections already wired. A team where each person runs a personal AIOS is a team that is genuinely AI-ready. Team features are on the roadmap; aios-starter is currently single-user.

---

## The Five Cs architecture

The system is organized around five layers, each building on the previous one.

| Layer | The question it answers | In place when |
|---|---|---|
| **Context** | Does it know you: your domains, voice, priorities, people? | A fresh session answers "what does this person do and what matters to them?" without re-briefing |
| **Connections** | Can it reach your actual tools and data? | "What's on my calendar tomorrow and what tasks are due?" returns live data, no paste required |
| **Capabilities** | Does it know how to do useful work for you specifically? | A short phrase triggers a multi-step workflow that produces the right output |
| **Cadence** | Does it run consistently without being pushed? | Regular patterns are running: weekly reviews, session captures, recurring checks |
| **Continuity** | Is it learning and improving over time? | Behavioral corrections accumulate; context stays fresh; the system gets better with use |

**Dependency:** Context is the foundation. Connections and Capabilities build in parallel. Cadence and Continuity come last. Do not automate what does not work manually, and do not try to improve a system that is not being used.

---

## Bucket architecture

Life does not run in a single context. This kit organizes your work around 2 to 4 parallel life domains, each with its own scoped context, each sharing a common skills layer and global persona.

Most people start with three buckets:

| Bucket | Typical scope |
|--------|--------------|
| personal | Life admin, finances, health, relationships, housing |
| career | Primary employment, consulting, or client work |
| venture | Independent projects, side business, or creative work |

Two buckets work for simpler setups. Four work for genuinely distinct domains. You name them during `/onboard`; the examples above are a starting point.

Context loads hierarchically: working from a bucket folder loads that bucket plus the root. Working from the root loads only the root. Cross-bucket context is always explicit. Never automatic.

---

## What ships

**Setup and growth skills:**

| Skill | When to run |
|---|---|
| `/onboard` | Day 1, immediately after clone. Intake interview, Day-1 file scaffold, and SOUL.md setup. Budget 15 to 20 minutes. |
| `/audit` | Monthly. Scores your system against the Five Cs. Surfaces the top gaps to close. |
| `/level-up` | Weekly. Find one friction, scope one fix, build one thing. |
| `/reflect` | Quarterly. Synthesizes audit history and patterns into one strategic recommendation. |

**Session continuity:** `retro` captures learnings and checkpoints `journal.md` at the end of each session. `handoff` produces a clean summary before clearing context.

**Thinking:** `grill-with-context` stress-tests a plan or decision before you commit to it.

**Build skills:** `skill-creator` builds new skills with a full test-and-eval loop. `skill-refine` makes targeted fixes to installed skills that are misfiring or producing wrong output.

**Document production:** `docx`, `pdf`, and `pptx` for Word, PDF, and PowerPoint tasks.

See `HANDBOOK.md` for the complete skill inventory with descriptions and trigger guidance.

---

## Quick start

1. Clone the repo to a working folder on your machine. If you work across multiple devices, consider placing your AIOS folder in Dropbox or a similar sync service before running setup; this keeps your files current everywhere.

2. Open your AIOS folder in your AI harness (Claude Code, Codex, or similar) and run `/onboard`. Answer the questions honestly. Voice samples must be pasted from real prior writing, not typed fresh. Budget 15 to 20 minutes. Your Day-1 file set and SOUL.md drop at the end.

3. Use it for a week with real work. Bring real questions. Log decisions by appending to `decisions.md`.

4. Day 7: run `/audit`. Read the Five-Cs gap report. Pick one gap to close.

5. Day 14: run `/level-up`. Find one recurring friction and build something that handles it.

6. Week 3 and beyond: weekly `/level-up`, monthly `/audit`, quarterly `/reflect`. Each pass makes the system more accurate and more autonomous.

The compounding is real but gradual. The system is most useful around week 4 to 6, when enough context, corrections, and decisions have accumulated to meaningfully shape its behavior.

---

## Repo layout

```
AIOS/
├── README.md
├── CLAUDE.md                        <- Root operating manual (loaded every session)
├── AGENTS.md -> CLAUDE.md           <- Symlink for cross-harness parity
├── SOUL.md                          <- Global persona (set up during onboard, loaded always)
├── HANDBOOK.md                      <- Reference manual: folders, conventions, skills
├── ROADMAP.md                       <- What to add as you grow, and when
├── LICENSE
├── .gitignore
├── journal.md                       <- Session diary
├── decisions.md                     <- Append-only decisions log
├── connections.md                   <- Registry of every tool your AIOS can reach
├── aios-intake.md                   <- Source of truth for /onboard
├── context/                         <- Cross-bucket static facts: voice, preferences
├── threads/                         <- Cross-bucket topic captures
├── artifacts/                       <- Cross-bucket outputs the system made
├── references/                      <- Inputs brought in from outside
├── learnings/                       <- Persistent behavioral corrections (root only)
├── archives/                        <- Retired content (root only)
├── audits/                          <- Saved /audit reports; score history
├── reflects/                        <- Saved /reflect reports; strategic history
├── templates/                       <- Reusable prompt templates
├── personal/                 <- Default bucket 1 (renamed during /onboard)
|   +-- context/  threads/  workstreams/  artifacts/  references/
|   +-- journal.md
├── career/                         <- Default bucket 2 (renamed during /onboard)
|   +-- (same layout)
├── venture/                  <- Default bucket 3 (renamed during /onboard)
|   +-- (same layout)
└── .agents/
    └── skills/                      <- (.claude/skills symlinks here)
        ├── onboard/       audit/          level-up/     reflect/
        ├── grill-with-context/            retro/        handoff/
        ├── skill-creator/ skill-refine/
        └── docx/          pdf/            pptx/
```

See `HANDBOOK.md` for the complete folder and file reference. See `ROADMAP.md` for what to add as you grow.

---

## Why this exists

This kit was built for people running multiple parts of their lives in parallel who wanted an AI that actually knows them, not one they have to re-brief every session.

The structure here reflects recognizable best practices for personal AI agent systems as of mid-2026. It is shared as a starting point so you do not have to build from scratch.

*With appreciation to Nate Herk, whose AIOS starter kit provided the original scaffold and foundational thinking this project builds on.*

---

## License

This project is licensed under Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0). You may use, adapt, and share it for personal and non-commercial purposes with attribution. Commercial use requires a separate arrangement.

See `LICENSE` for the full license text.
