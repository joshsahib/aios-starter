# AIOS Intake

This is the source-of-truth file for your AIOS. Fill it in by typing, voice dictation, or running `/onboard` for a guided conversation. Whichever mode, this file is what `/onboard` reads to scaffold your Day-1 file set.

**Hard cap: 7 questions.** Each answerable in under 90 seconds. Don't overthink — you can edit and re-run `/onboard` any time to update your files.

**Bucket concept:** This kit is organized around three life domains, or *buckets* — personal-family, day-job, and business-hobby. Most questions ask you to cover all active buckets. Use your own labels where the defaults don't fit, but try to map your domains to one of the three.

---

## Q1 — Who are you and what are your main domains?

Briefly describe yourself and list your active life domains. Map them to the three buckets where you can (personal-family, day-job, business-hobby). One or two sentences per domain — what it is, your role in it, and roughly how much time and energy it demands.

```
[Your answer here]
```

---

## Q2 — Paste 1–2 things you've written recently. Don't edit them.

An email, a message, a doc, a post — anything that sounds like you when you're not performing. **Paste verbatim.** Do not type these mid-conversation with Claude — samples written during the conversation are already shaped by that context, making them less useful as voice references.

```
[Sample 1 — paste raw]
```

```
[Sample 2 — paste raw]
```

---

## Q3 — What are your biggest priorities for the next 90 days?

List 2–4 things that, if not done by then, would make you feel like the quarter was wasted. These can span multiple buckets. Be specific — not "grow the business" but "close 3 enterprise deals" or "launch by April 30." Label each with its bucket if they span domains.

```
1. [Priority 1] ([bucket])
2. [Priority 2] ([bucket])
3. [Priority 3 — optional] ([bucket])
4. [Priority 4 — optional] ([bucket])
```

---

## Q4 — Where does money come in and go out, and how do you track it?

Cover all buckets with financial activity. Income sources, payment processors, expense tracking tools, accounting software, spreadsheets — whatever you actually use. Include both personal finance and any business/side project revenue. "I don't really track it well" is a valid answer.

```
[Your answer here]
```

---

## Q5 — Where do you communicate day-to-day, across all your domains?

Email (which accounts and clients), messaging apps (Slack, Teams, Discord, iMessage), any platforms where you interact with customers, teammates, or the outside world. Break it down by bucket if the tools differ significantly. Include your calendar setup here too.

```
[Your answer here]
```

---

## Q6 — Where does information live, and what AI tools do you use?

**Part A — Information landscape:** Meeting notes, recordings, documents, reference files, knowledge bases. Include everything even if it's messy or scattered — this helps the system understand what exists and where to look.

**Part B — AI stack:** What AI tools, services, or subscriptions do you currently use or pay for? (Examples: Claude, ChatGPT, Perplexity, Cursor, Zapier AI, Make, specific LLM APIs, etc.) Include anything you actively use, even if occasionally.

```
Part A — Information landscape:
[Your answer here]

Part B — AI stack:
[Your answer here]
```

---

## Q7 — What's your biggest recurring friction, and where do you track tasks?

The one thing that reliably eats your time or creates drag each week. And separately: where do your tasks and projects actually live?

```
top_friction — describe the recurring problem:
[Your answer here]

task_system — tool or method you currently use:
[Your answer here]
```

---

When this file is complete, run `/onboard` (or re-run it) and the skill will scaffold your Day-1 file set: root `context/`, bucket-level `context/` files, a populated `connections.md`, and a filled `CLAUDE.md`.
