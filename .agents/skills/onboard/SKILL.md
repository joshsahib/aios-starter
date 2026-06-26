---
name: onboard
description: Use on Day 1 of an AIOS install, when someone says "set me up", "onboard me", "let's get started", "fill in my AIOS", or has just cloned the kit. Combined wizard — runs the intake interview AND scaffolds the Day-1 file set at the end. Idempotent — re-run any time after editing aios-intake.md to refresh context files.
---

## What this skill does

Single combined wizard. Reads or writes `aios-intake.md` (the canonical intake), conducts the interview if the file is not filled, then scaffolds the Day-1 file set at the end of the run.

This skill is designed for the **three-bucket model**: personal-family, day-job, and business-hobby. It creates context at the right level — cross-domain information (voice, preferences) at root, domain-specific information inside each bucket's `context/` folder. It also handles the companion setup files: `SOUL.md` prompts, `journal.md` initialization, and `AGENTS.md` symlinks.

## When NOT to run this

- If the user wants to add a new tool connection: point them to `connections.md` to edit directly.
- If the user wants a deeper review of their setup after Day 14: that is `/audit` territory.
- If a specific project context file needs updating: do that directly rather than re-running full onboard.

## Execution

### Step 1: Read the intake and assess

Read `aios-intake.md`. Check which Q1–Q7 sections have content vs. `[Your answer here]` placeholders.

- **All filled** → skip Step 2, jump to Step 3 (scaffold).
- **Some filled** → tell the user which questions are answered, which are not. Ask: "Want to fill the rest now, or scaffold from what's there?" Their call.
- **None filled (fresh clone)** → run Step 2 conversationally.

Before starting, ask one scoping question:

> *"Is this a full AIOS setup across all three buckets (personal-family, day-job, business-hobby), or are you onboarding into one specific bucket? For most people starting out, full setup is the right call — but if you're adding a new bucket to an existing system, say so."*

If full setup: proceed as below. If bucket-specific: scope Step 3 to write into that bucket's directory only.

### Step 2: The interview (7 questions, hard cap)

Ask one at a time. Write each answer into `aios-intake.md` as you go so the user can resume if interrupted. Keep the tone conversational — this is a setup interview, not a form.

---

**Q1 — Who are you and what are your main domains?**

You are trying to understand: the person's identity, what major areas of life or work they are actively running, how they describe those domains, and roughly how their time is split. Aim to map their domains to the three-bucket model (personal-family, day-job, business-hobby) — but use their language, not yours. Two or three sentences per domain is enough. Push back gently if they are too vague — "I have a job and some side stuff" is not enough to work with.

---

**Q2 — Paste 1–2 things you've written recently. Don't edit them.**

*This is the one question with a hard rule.* Voice samples must be pasted from real writing — not typed mid-conversation. If the user starts typing fresh prose, stop them:

> *"Pause — I need you to paste something you wrote before this conversation, not draft something new right now. Even good writers produce different text when they know an AI is watching. Open a recent email, message, or doc in another tab and paste it verbatim. This is the one thing I can't be flexible on."*

Ask for one or two samples. An email and a post is ideal. Two emails works. Two typed-in-chat samples do not.

---

**Q3 — What are your biggest priorities for the next 90 days?**

You want 2–4 specific, time-bound outcomes — things that, if not done by the end of the quarter, would feel like a failure. Push back on vague answers:

- "Grow my business" → "What does growth look like specifically? A revenue number? A customer count? A product shipped?"
- "Get more organized" → "Organized enough to do what? What breaks right now because things aren't organized?"

Priorities can span multiple domains — that's fine and often more honest than pretending each domain has its own separate priority list. Label each priority with its bucket when you scaffold it.

---

**Q4 — Where does money come in and go out, and how do you track it?**

Cover all domains with financial activity. You want: income sources, payment processors (Stripe, PayPal, direct invoice), expense tracking (QuickBooks, spreadsheet, Monarch, nothing), and whether there is a distinction between personal and business finances. "I don't track it well" is a legitimate answer — note it, because the system can help.

---

**Q5 — Where do you communicate day-to-day?**

Map their communication landscape across all domains: email accounts and clients, messaging platforms (Slack, Teams, Discord, iMessage), customer-facing platforms, team tools. If different domains use significantly different tools, note which tool belongs to which domain.

Calendar is typically inferred from Q5: Gmail → Google Calendar; Outlook → Outlook Calendar. Confirm this assumption at the end of Q5 before moving on.

---

**Q6 — Where does information live, and what AI tools do you use?**

Two parts:

1. **Information landscape:** Meeting notes and recordings, documents, reference files, knowledge bases — everything, even if it is scattered. You want to understand what exists and where it lives so `connections.md` can be populated accurately and the system knows where to look for context. "It's a mess" is a valid answer.

2. **AI stack:** What AI tools, services, or subscriptions do you currently use or pay for? (Examples: Claude, ChatGPT, Perplexity, Cursor, Zapier AI, Make, specific LLM APIs, custom GPTs, etc.) This helps understand what's already in the operator's toolkit so the AIOS doesn't duplicate work or suggest tools they already have. Include anything you actively use, even if just occasionally.

---

**Q7 — What's your biggest recurring friction, and where do you track tasks?**

Two parts:

1. The one thing that reliably drains time or creates drag every week. This becomes `top_friction` in the context files and seeds future `/level-up` and `/audit` sessions.
2. Where tasks and projects actually live — Linear, Todoist, Notion, a notebook, their head, or some combination.

---

### Step 3: Scaffold the Day-1 file set

Once the intake is complete, generate or update the following files. If files already exist, back up originals to `archives/intake-{YYYY-MM-DD-HHMM}/` before overwriting.

#### Root-level files (always)

1. **`context/about-me.md`** — From Q1 (identity and cross-domain summary) and Q7 (top_friction). One paragraph on who the person is, one paragraph on their domains, one sentence on the recurring problem they are trying to solve. This file is cross-domain — it describes the whole person, not one bucket.

2. **`context/voice.md`** — From Q2. Paste samples verbatim under headers that describe their origin ("Recent email to a client", "LinkedIn post — Feb 2026"). Add a one-line note at the top: *"Match this register when drafting. Flag it if you think a draft significantly departs from this voice."*

3. **`connections.md`** — Populate from Q4–Q7. One row per tool or platform. Columns: domain, tool name, purpose, mechanism (not yet connected / MCP / API / manual), auth status, last checked. Initialize all mechanism fields as `not yet connected` — the user wires actual connections on Day 2+. Include AI tools from Q6 as a separate section.

4. **`CLAUDE.md`** — Fill all `{{...}}` placeholders. Include: user's name and identity summary (Q1), domain list with one-line descriptions mapped to buckets, current top priorities (Q3) labeled by bucket, voice register summary (from Q2 samples — describe the register, do not paste full samples here), and a connections summary pointing to `connections.md`.

5. **`journal.md`** — If not already present, initialize with the header format from the template. Do not overwrite if it already has entries.

6. **`SOUL.md`** — If not already filled (still contains `[bracketed placeholders]`), prompt the user to fill it in after onboarding. Note in the closing screen: "SOUL.md is your global persona — fill it in before your next session."

#### Bucket-level files (for each active domain)

For each named bucket that exists as a directory (`personal-family/`, `day-job/`, `business-hobby/`):

1. **`[bucket]/context/priorities.md`** — Numbered list of priorities from Q3 that belong to this domain. One line per priority. If priorities span buckets, add a note in each relevant bucket's file.

2. **`[bucket]/context/about-[bucket].md`** — Domain-specific context: what this domain is, the user's role in it, key tools from Q4–Q6 that belong here, and any domain-specific friction from Q7. Keep it tight — two to three short paragraphs.

3. **`[bucket]/CLAUDE.md`** — Create only if the user explicitly requests it or if the bucket has substantially different context than the root would suggest. Per the kit design: don't create preemptively. If you do create it, keep it lean at Day 1. Note its creation in the closing screen.

4. **`[bucket]/AGENTS.md → CLAUDE.md` symlink** — Create this symlink when you create a bucket-level `CLAUDE.md`. Command: `ln -s CLAUDE.md [bucket]/AGENTS.md`.

**Do not create per-domain directories yourself.** Write into existing structure only. Note recommended next structural steps as suggestions in the closing output, not by creating files unilaterally.

### Step 4: The closing screen

Print one screen. Keep it short:

```
✓ Day 1 done.

Your AIOS now knows who you are, what buckets you're running, what matters this quarter, and how you write.

Next up:
• Fill in SOUL.md — your global persona. It loads in every session.
• Open connections.md and wire up one tool.
• Day 7: run /audit to score your setup.
• Day 14: run /level-up to find your first automation.

Try this now: ask me — "what should I focus on this week?"
```

When the user runs the "what should I focus on this week?" prompt, respond using only the newly scaffolded context files. The response should:
- Open with a 3–4 bullet priority list drawn directly from `context/priorities.md` and bucket-level `context/priorities.md` files, labeled by bucket
- Match the voice register from `context/voice.md` — not generic AI output
- End with: *"If I had to pick one thing for this week, it's [X], because [reason grounded in their stated priorities]. Want me to draft the first move?"*

## Critical implementation rules

1. **The 7-question cap is non-negotiable.** Do not add an eighth question in conversation. If something comes up that feels important, fold it into an existing question's answer or add a note to the scaffolded files for the user to review.

2. **Voice paste cannot be skipped or substituted.** If the user types samples mid-chat, refuse and explain why. If they genuinely have nothing to paste right now, accept one sample and note in `context/voice.md` that a second sample should be added before relying heavily on voice-matched drafts.

3. **One-shot scaffold.** After Step 2, write all Step 3 files in a single batch. No multi-turn confirmation for each file. The user iterates by editing `aios-intake.md` and re-running.

4. **Idempotent.** Re-running with an edited intake refreshes context files; backs up originals to `archives/intake-{ts}/`. Skip questions already answered unless the user explicitly wants to revise them.

5. **Closing screen is short.** Four to five lines. Not a menu, not a list of every skill available.

6. **Do not scaffold directories that do not exist.** Write into existing structure only. Note recommended next structural steps as suggestions in the closing output, not by creating files unilaterally.

7. **No API key requests on Day 1.** Connections come Day 2+. Do not ask for credentials during onboarding.

8. **No `.env` writes.** Ever.

9. **Bucket priorities go in bucket context, not just root.** Cross-domain priorities can appear in multiple places, but each bucket's `context/priorities.md` should contain what's relevant to that domain.

## Verification

- **Cold test:** Clone a fresh kit, run `/onboard`, fill 7 answers, confirm scaffold runs across root + bucket levels, ask the closing prompt, verify response cites Q1 + Q3 + Q7 specifically. Generic or hallucinated output = fail.
- **Idempotency test:** Re-run `/onboard` with one Q3 priority changed. Expected: only `context/priorities.md` and bucket-level priority files update; backup created in `archives/intake-{ts}/`.
- **Voice rejection test:** Type a sample mid-chat. Expected: skill refuses, asks for paste from real writing.
- **Bucket-aware test:** Run with three distinct domains. Expected: each active bucket directory gets its own `context/priorities.md` and `context/about-[bucket].md`; root `context/about-me.md` describes the whole person.
- **AI stack test:** Q6 answers include AI tools. Expected: AI tools appear in `connections.md` under their own section; not conflated with data connections.
