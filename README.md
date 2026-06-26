# AIOS — AI Operating System starter kit for Claude Code

A free, MIT-licensed starter kit that turns Claude Code into your personal **AI Operating System (AIOS)**. Designed for anyone running multiple life domains in parallel — professionals, operators, founders, creators — who want their AI to know who they are, what matters, and how they work across all of it.

The kit personalizes itself to you via `/onboard`, then gives you three recurring skills (`/audit`, `/level-up`, `/grill-with-context`) to keep building leverage week over week.

---

## The litmus test

> **"While you're not at your desk, your AIOS observes one real-world event and produces an output that's faster and more accurate than what you'd produce yourself."**

Every design decision in this kit rolls up to that test. If a layer, skill, or template doesn't contribute to it, it doesn't ship.

---

## How you'll know it's working

Three felt **success indicators** tell you the AIOS is actually changing how you work. Not KPIs — there's no objective metric. These are lived experiences that show up in your week.

**1. Team-reaches-out:**

> *"A teammate messages you with a question. You realize your AIOS would answer it better, faster, and with exact sources — even if you were awake and free. So you ask your AIOS too. That's the moment you stop being a bottleneck for your own knowledge."*

**2. Context-switching reduction:**

> *"You stop opening new tabs. You stop launching the desktop app. When something new lands, your first move is to ask the AIOS, not to open six things. The default surface for thought work shifts. Silent. Compounding."*

**3. Knowledge-leaves-your-head:**

> *"You stop trying to remember business facts. You don't rehearse what you decided last quarter or what your customer said in that meeting. You trust the retrieval. The AIOS holds the truth, you hold the questions."*

**Personal foundation → company AI-readiness.** Once these indicators show up for one person, the same data architecture powers everything else. Custom dashboards on the data you already collect. Automations on top of the connections you already wired. Team rollout where everyone has theirs. *A company where every operator runs a personal AIOS is a company that's actually AI-ready.*

The kit teaches personal AIOS first. Everything scales from there.

---

## Two frameworks

The kit teaches two complementary frameworks. **Three Ms first, Four Cs second.** Without the brain rewire, the architecture is just a folder structure.

### The Three Ms — operator brain (how you think)

| M | One-liner |
|---|---|
| **Mindset** | Default Shift, Function Breakdown, Curiosity Rule. *To what extent can AI be leveraged here?* |
| **Method** | Find Constraint → EAD (Eliminate, Automate, Delegate) → Map Process → Pick Autonomy Level → Tie to KPI. |
| **Machine** | Lego Principle, Validation Chain, Bike Method, Intern Rule, Kill Switch. *Boring is beautiful. Workflows beat agents.* |

Full breakdown in `references/3ms-framework.md`. The `/level-up` skill walks you through all three weekly.

### The Four Cs — architecture (what you build)

| # | Layer | One-liner | "This layer is in place" test |
|---|---|---|---|
| 1 | **Context** | Knows your life and work | Fresh Claude session answers "what does this person do and what matters to them?" without browsing |
| 2 | **Connections** | Reaches your stuff | "What's on my calendar tomorrow and what tasks are due?" → live data, no paste |
| 3 | **Capabilities** | Knows how to do the work | A short phrase triggers a multi-step workflow that produces an artifact |
| 4 | **Cadence** | Runs without being asked | Laptop closed. A brief lands in the inbox. A teammate messages it and gets a real answer |

**Brand line:** Context. Connections. Capabilities. Cadence.

Dependency graph: Context is non-skippable. Connections + Capabilities can build in parallel. Cadence is last — don't automate workflows that don't work manually.

---

## Three-bucket architecture

Life doesn't run in a single context. This kit is structured around three parallel life domains — each with its own scoped context and brainstorm space, all sharing a common skills layer and global persona.

| Bucket | Default scope |
|--------|--------------|
| `personal-family/` | Life admin, finances, health, relationships, housing, travel |
| `day-job/` | Primary employment — role, employer, projects, obligations |
| `business-hobby/` | Independent ventures, side projects, creative work |

Claude Code loads context hierarchically: working from the bucket folder loads that bucket's context plus the root. Working from the root loads only the root. Cross-bucket context is always explicit — via `@include`, a skill, or direct instruction. Never automatic.

---

## What ships — skills

The kit ships four recurring AIOS skills — ideation prompts and thinking tools, not heavy automations — plus a set of bundled utility skills. You hack on top of the structure.

| Skill | Type | When to run |
|---|---|---|
| `/onboard` | Setup wizard (one-time) | Day 1, immediately after clone. Interview + scaffold. Generates Day-1 file set. |
| `/grill-with-context` | Adversarial pre-flight | Before any significant plan or decision. Stress-tests assumptions before you commit. |
| `/audit` | Recurring thinking skill | Day 7, then weekly. Four-Cs gap report. Read-only. Watch the score climb. |
| `/level-up` | Recurring thinking skill | Day 14, then weekly. Three Ms interview. One run = one shipped artifact. |

**Bundled utility skills:** `retro` and `handoff` (session continuity), `skill-creator` (build/optimize skills), and `docx` / `pdf` / `pptx` (document generation).

`/audit` asks *"is the AIOS built right?"* (form). `/level-up` asks *"what leverage am I missing?"* (function). They work in series — fix structure first, then capability planning becomes meaningful.

---

## Quick start

1. **Clone the repo** to a working folder on your machine.
2. **Open it in Claude Code** and run `/onboard`. Answer the questions honestly. Voice samples must be pasted, not described. Takes ~15 minutes. Day-1 file set drops at the end.
3. **Use it for a week.** Bring real questions. Make real decisions. Log them by appending to `decisions.md`.
4. **Day 7:** run `/audit`. Read the Four-Cs gap report. Pick one gap to close.
5. **Day 14:** run `/level-up`. The Three Ms interview surfaces one automation worth building. Build it.
6. **Week 3+:** weekly `/level-up` ritual. One shipped artifact per week.

---

## Repo layout

```
AIOS/
├── README.md
├── CLAUDE.md                        ← Root operating manual (loaded in every session)
├── AGENTS.md → CLAUDE.md            ← Symlink for cross-harness parity
├── SOUL.md                          ← Global persona (fill once, loaded always)
├── HANDBOOK.md                      ← How the system works: folders, conventions, workflows, skills
├── ROADMAP.md                       ← What to add as you grow, and when
├── LICENSE
├── .gitignore
├── journal.md                       ← Session diary; cross-bucket summary per session
├── decisions.md                     ← Append-only record of what was decided and why
├── connections.md                   ← Registry of every system your AIOS can reach
├── aios-intake.md                   ← Source-of-truth for /onboard. Edit + re-run any time.
├── context/                         ← Cross-bucket static facts: voice samples, global preferences
├── threads/                         ← Cross-bucket topic captures (one file per topic)
├── artifacts/                       ← Cross-bucket outputs the system made
├── references/                      ← Inputs brought in from outside
│   └── 3ms-framework.md             ← The operator brain (Three Ms framework)
├── learnings/                       ← Persistent behavioral corrections (root-level only)
├── archives/                        ← Retired content. Move here; don't delete.
├── personal-family/                 ← Life admin, finances, health, relationships
│   ├── context/  threads/  workstreams/  artifacts/  references/
│   └── journal.md  decisions.md     ← (+ CLAUDE.md & AGENTS.md only when needed)
├── day-job/                         ← Primary employment
│   ├── context/  threads/  workstreams/  artifacts/  references/
│   └── journal.md  decisions.md
├── business-hobby/                  ← Independent ventures, side projects
│   ├── context/  threads/  workstreams/  artifacts/  references/
│   └── journal.md  decisions.md
└── .agents/
    └── skills/                      ← (.claude/skills → ../.agents/skills symlink)
        ├── onboard/   audit/   level-up/   grill-with-context/
        ├── retro/     handoff/   skill-creator/
        └── docx/      pdf/       pptx/
```

See `ROADMAP.md` for what to add as you grow.

---

## Why this exists

I built this while running several parts of my life in parallel, and I got tired of re-explaining who I am and what I'm working on to an AI at the start of every session. I'm sharing it so you don't have to start from scratch. Used with the right intentions, AI is a genuine force multiplier — better decisions, clearer thinking, more intentional work, not just faster output. The structure here reflects what have become industry best practices for AI agent systems as of mid-2026. Special thanks to Nate Herk, whose AIS-OS starter kit provided the original scaffold and a good deal of the thinking this fork builds on.

---

## License + attribution

MIT License. Copyright (c) 2026 Joshua Sahib.

This project was inspired by and adapted from a personal AIOS starter kit originally created by Nate Herk, which introduced the Four Cs of an AIOS and the Three Ms of AI™ as its conceptual foundation. This fork has been substantially reworked for a multi-domain, three-bucket use case. The Three Ms of AI™ is a trademark of Nate Herk; the framework is included in `references/3ms-framework.md` with full attribution.
