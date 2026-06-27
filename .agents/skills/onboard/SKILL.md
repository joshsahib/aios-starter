---
name: onboard
description: First-run wizard for AIOS setup. Trigger on: "set me up", "onboard me", "let's get started", "walk me through setup", "I just cloned this", "initialize my system", "I'm starting from scratch", or any first-session state with no context files populated. Establishes bucket names, runs the intake interview, and scaffolds the Day-1 file set. Idempotent: re-run after editing aios-intake.md to refresh context files.
---

## What this skill does

Single combined wizard. Runs in three phases:

**Phase 1 — Bucket setup.** Confirms or establishes the user's domain names before anything else. Renames placeholder directories, updates root CLAUDE.md, and records confirmed bucket names at the top of aios-intake.md.

**Phase 2 — Intake and scaffold.** Reads or writes aios-intake.md, conducts the 9-question interview if not already filled, then scaffolds the Day-1 file set across root and bucket levels.

**Phase 3 — SOUL.md setup.** Walks through the global persona file immediately after the scaffold, so the system is fully configured before the first real session begins.

This skill is designed for a multi-bucket AIOS model. It creates context at the right level — cross-domain information (voice, preferences) at root, domain-specific information inside each bucket's context/ folder.

## When NOT to run this

- If the user wants to add a new tool connection: point them to connections.md to edit directly.
- If the user wants a deeper review of their setup after Day 14: that is /audit territory.
- If a specific context file needs updating: do that directly rather than re-running full onboard.

## Execution

### Phase 1: Establish bucket names

Before the interview begins, verify that the user's bucket structure is named correctly.

Read the root CLAUDE.md bucket table. Check whether directory names are still generic defaults (personal/, career/, venture/).

**If buckets already have custom names:** Note them and proceed to Phase 2.

**If buckets are still generic defaults:** Say:

> *"Before we start, let's name your domains.*
>
> *Your AIOS is organized around buckets — the 2–4 major areas your life actually runs on. Think of them as the big chapters: something like 'personal', 'career', and 'startup' — or 'family', 'freelance', 'creative'. The test: if someone asked what you do, and you'd answer in two or three words, those are probably your buckets.*
>
> *A few things buckets are NOT: a specific project, a client, a topic, or a phase of life. Those live inside a bucket. Three buckets is the most common setup, but two is fine if that's honest, and four works if your life genuinely runs in four distinct domains. More than four usually means over-organizing.*
>
> *What are your main domains, and what would you call them?"*

Once the user responds:
1. Confirm the final list: "So we will set up [N] buckets: [name1], [name2], [name3]. Ready to proceed?"
2. Wait for confirmation.
3. Programmatically rename directories, update the bucket table in root CLAUDE.md, and record confirmed names at the top of aios-intake.md. Complete all three before proceeding.
4. Confirm to the user: "Done — your buckets are set up. Let us start the interview."

**Do not begin the interview until bucket names are confirmed and directories are renamed.**

---

### Phase 2: Intake and scaffold

#### Step 1: Read the intake and assess

Read aios-intake.md. Check which Q1 through Q9 sections have content vs. placeholder text.

- **All filled** — skip Step 2, jump to Step 3 (scaffold).
- **Some filled** — tell the user which questions are answered, which are not. Ask: "Want to fill the rest now, or scaffold from what's there?" Their call.
- **None filled** — run Step 2 conversationally.

Before starting, ask one scoping question:

> *”Is this a full AIOS setup across all your buckets, or are you setting up one specific bucket? For most people starting out, full setup is the right call — but if you're adding a new bucket to an existing system, say so.”*

If full setup: proceed as below. If bucket-specific: scope Step 3 to write into that bucket's directory only.

#### Step 2: The interview (9 questions, hard cap)

Ask one at a time. Write each answer into aios-intake.md as you go so the user can resume if interrupted. Keep the tone conversational — this is a setup interview, not a form. Aim for 60 to 90 seconds per question.

---

**Q1 — Who are you and what are your main domains?**

Understand the person's identity, their active domains, and how they describe them. Map answers to the confirmed bucket names from Phase 1. Two or three sentences per domain is enough — what it is, their role in it, and roughly how much time and energy it demands. Push back gently if they are too vague.

---

**Q2 — Paste 1 to 2 things you have written recently. Do not edit them.**

This is the one question with a hard rule. Voice samples must come from real prior writing — not typed mid-conversation. If the user starts typing fresh prose, stop them:

> *"Pause — I need you to paste something you wrote before this conversation, not draft something new right now. Open a recent email, message, or doc and paste it verbatim. Even a short one works. This is the one thing I can't be flexible on — samples written during a conversation are already shaped by that context, which makes them less useful as voice references."*

An email and a message is ideal. Two emails works. Two typed-in-chat samples do not. If the user genuinely has nothing to paste right now, accept one sample and note in context/voice.md that a second sample should be added before relying heavily on voice-matched drafts.

---

**Q3 — What are your biggest priorities for the next 90 days?**

You want 2 to 4 specific, time-bound outcomes — things that, if undone by end of quarter, would feel like a failure. Push back on vague answers:

- "Grow the business" — "What does growth mean specifically? A revenue target? Customer count? A product shipped by a date?"
- "Get more organized" — "Organized enough to do what? What breaks right now because things are not organized?"

Priorities can span domains — label each one with its bucket. That is how they get scaffolded.

---

**Q4 — Who are the key people in each of your domains?**

A short roster of people who show up regularly across your buckets. First name and role — last name only if needed for disambiguation. Aim for 3 to 5 per bucket. The goal is coverage, not a complete org chart.

Prompt with an example:

> *"Something like — Personal: Alex (partner), Jordan (sibling). Work: Sam (your manager), Taylor (direct report). Startup: Casey (co-founder), River (lead engineer). Just the people who come up most often."*

---

**Q5 — Where do you communicate day-to-day?**

Map the communication landscape across all domains: email accounts and clients, messaging platforms (Slack, Teams, Discord, iMessage), customer-facing platforms, team tools. If different domains use significantly different tools, note which belongs where.

Calendar is typically inferred: Gmail maps to Google Calendar, Outlook maps to Outlook Calendar. Confirm this assumption before moving on.

---

**Q6 — Where does information live, and what AI tools do you use?**

Two labeled parts — ask for both:

**Part A — Information landscape:** Meeting notes and recordings, documents, reference files, knowledge bases. Include everything, even if it is scattered or messy. You want to understand what exists and where so connections.md can be populated accurately.

**Part B — AI stack:** What AI tools, services, or subscriptions do you currently use or pay for? Include anything actively used, even occasionally. Examples: Claude, ChatGPT, Perplexity, Cursor, Zapier AI, Make, custom GPTs, specific LLM APIs.

---

**Q7 — Where do tasks and projects actually live?**

Not the ideal system — the actual one. Task managers, project tracking tools, and whether different domains use different systems. "It is mostly in my head" is a legitimate answer — note it. This determines where the AIOS can surface context most usefully and feeds future /level-up planning.

---

**Q8 — What is your biggest recurring friction?**

The one thing that reliably drains time or creates drag each week. This becomes top_friction in context files and seeds future /level-up and /audit sessions.

Push back on vague answers:
- "I am always busy" — "Busy doing what specifically? What type of work takes more time than it should?"
- "Staying on top of things" — "Which things — communication, decisions, project tracking, something else?"

---

**Q9 — Where does money come in and go out?**

Scale depth to complexity. Open with:

"If your finances are simple — one salary, personal spending — one line is enough. If you have business income, multiple revenue streams, or financial activity across buckets, go a bit deeper."

Cover: income sources, payment processors, expense tracking tools, and whether personal and business finances are separated. "I do not track it well" is a valid answer — note it, because the system can help.

---

#### Step 3: Scaffold the Day-1 file set

Once the intake is complete, generate or update the following files in a single batch. If files already exist, back up originals to archives/intake-{YYYY-MM-DD-HHMM}/ before overwriting.

**Root-level files (always)**

1. **context/about-me.md** — From Q1 and Q8 (top_friction). One paragraph on who the person is, one paragraph on their domains, one sentence on the recurring problem they are trying to solve. Cross-domain — describes the whole person, not one bucket.

2. **context/voice.md** — From Q2. Paste samples verbatim under headers describing their origin. Add a note at the top: "Match this register when drafting. Flag it if a draft significantly departs from this voice."

3. **connections.md** — From Q5 through Q9. One row per tool or platform. Columns: domain, tool name, purpose, mechanism (not yet connected / MCP / API / manual), auth status, last checked. AI tools from Q6 Part B appear in a separate section. Initialize all mechanism fields as not yet connected.

4. **CLAUDE.md** — Fill all placeholders. Include: name and identity summary (Q1), domain list mapped to confirmed buckets, current top priorities (Q3) labeled by bucket, voice register summary (from Q2 — describe the register, do not paste full samples here), connections summary pointing to connections.md.

5. **journal.md** — If not already present, initialize with the standard header format. Do not overwrite if it already has entries.

6. **SOUL.md** — Do not modify during the scaffold step. Phase 3 handles SOUL.md setup immediately after the closing screen.

**Bucket-level files (for each active bucket)**

For each confirmed bucket directory:

1. **[bucket]/context/priorities.md** — Priorities from Q3 that belong to this domain. One line per priority. If a priority spans buckets, note it in each relevant file.

2. **[bucket]/context/about-[bucket].md** — Domain-specific context: what this domain is, the user's role in it, key tools from Q5 through Q6 that belong here, any domain-specific friction from Q8. Two to three short paragraphs.

3. **[bucket]/context/people.md** — From Q4. One entry per person in this domain: first name, role, one-sentence relationship note. Keep it flat and scannable.

4. **[bucket]/context/finances.md** — Created only if Q9 includes meaningful financial activity for this domain (business income, multiple revenue streams, or financial tools specific to this bucket). For simple personal finances, a paragraph in root about-me.md is sufficient — do not create this file unnecessarily.

5. **[bucket]/CLAUDE.md** — Create when the bucket has genuinely distinct operational context that differs meaningfully from root. When created, note it in the closing screen.

6. **[bucket]/AGENTS.md symlink to CLAUDE.md** — Create when a bucket-level CLAUDE.md is created. Command: ln -s CLAUDE.md [bucket]/AGENTS.md

**Do not create directories that do not exist.** Write into existing structure only. Note recommended next structural steps as suggestions in the closing screen, not by creating files unilaterally.

#### Step 4: The closing screen

Print one screen. Keep it short:

```
✓ Day 1 is mostly done.

Your AIOS now knows who you are, what buckets you're running,
what matters this quarter, and how you write.

Next up:
• Fill in SOUL.md — your global persona. It loads in every session.
• Open connections.md and wire up one tool.
• Day 7: run /audit to score your setup.
• Day 14: run /level-up to find your first automation.

```

When the user runs the "what should I focus on this week?" prompt, respond using only the newly scaffolded context files. The response should:
- Open with a 3–4 bullet priority list drawn from `context/priorities.md` and bucket-level `context/priorities.md` files, labeled by bucket
- Match the voice register from `context/voice.md` — not generic AI output
- End with: *"If I had to pick one thing for this week, it's [X], because [reason grounded in their stated priorities]. Want me to draft the first move?"*

---

### Phase 3: SOUL.md setup

SOUL.md is your global persona — it shapes tone, format, decision-making style, and behavioral rules across every session. It loads in every context. Complete this phase now rather than deferring — a system without a configured SOUL.md starts cold every session.

Check SOUL.md first. If it still contains the options template (A/B/C choices per section), run setup now. If it has already been collapsed to chosen preferences, skip this phase.

**Pre-populate Identity from the intake.** Before asking any questions, read aios-intake.md. Pre-fill the Name, Roles, and Building toward fields in the Identity section from Q1 and Q3 answers already captured there. Show the pre-filled version to the user and ask: "Does this capture who you are and what you are working toward, or would you like to adjust it?" Accept their edits. Do not re-ask Q1 or Q3 from scratch — the intake is the source of truth and this should feel like confirmation, not duplication.

Walk through each remaining section one at a time. Present the options as written and ask the user to choose A, B, or C — or write their own. Keep it conversational; the whole phase takes 3 to 5 minutes.

Sections in order:
1. **Identity** — confirm or adjust the pre-filled name, roles, and direction.
2. **Communication preferences** — Directness, Pushback, Format, Avoid. For the first three, present A/B/C with the default noted. For Avoid, show the defaults and ask if anything should be removed or added.
3. **Decision-making heuristics** — Options presentation, Uncertainty, Iteration style. Same pattern.
4. **Confirmation rules** — present the defaults; ask if anything should be added to either list.
5. **Caution triggers** — present the universal defaults; ask if there are locked decisions or domain-specific triggers to add.

After all five sections, write a collapsed SOUL.md containing only the chosen preferences. Rules for the collapsed file: no options lists, no A/B/C labels, no default notes, no template comments. Each section heading stays; only the chosen content appears below it. Aim for under one page. Write each preference as a concise statement, not a copy of the option text.

Back up the original template to archives/soul-template-{date}.md before overwriting.

Close Phase 3 with:

---
```
✓ SOUL.md is set up - Day 1 tasks complete.

The system now knows how you want to work — your communication style,
decision-making preferences, and standing rules load in every session.

Your AIOS is ready to use.
```
---

## Critical implementation rules

1. **Phase 1 before Phase 2 before Phase 3.** Never begin the interview before bucket names are confirmed. Never skip Phase 3 — it completes the setup.

2. **The 9-question cap is non-negotiable.** Do not add a tenth question. If something important comes up mid-interview, fold it into an existing answer or note it in the scaffolded files.

3. **Voice paste cannot be skipped or substituted.** If the user types samples mid-chat, refuse and explain why. If they genuinely have nothing to paste right now, accept one sample and flag the gap in context/voice.md.

4. **One-shot scaffold.** After Step 2, write all Step 3 files in a single batch. No multi-turn confirmation for each file. The user iterates by editing aios-intake.md and re-running.

5. **Idempotent.** Re-running with an edited intake refreshes context files and backs up originals to archives/intake-{ts}/. Skip questions already answered unless the user explicitly wants to revise them.

6. **Closing screen is short.** Three bullets. Not a menu, not a feature list.

7. **Do not create directories.** Write into existing structure only.

8. **No API key requests on Day 1.** Connections come Day 2+.

9. **No .env writes.** Ever.

10. **finances.md is conditional.** Create per-bucket only if Q9 reveals meaningful financial activity for that domain. Simple personal finances go in about-me.md as a paragraph.
