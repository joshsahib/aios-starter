# AIOS Intake

This is the source-of-truth file for your AIOS. Fill it in by typing, voice dictation, or running `/onboard` for a guided conversation. Whichever mode you use, this file is what `/onboard` reads to scaffold your Day-1 file set.

**9 questions.** Each answerable in under 90 seconds. Don't overthink — you can edit and re-run `/onboard` any time to update your files.

---

## Your Buckets

*Set automatically during Phase 1 of onboarding. Do not edit manually.*

```
Bucket 1: [name]
Bucket 2: [name]
Bucket 3: [name — if applicable]
Bucket 4: [name — if applicable]
```

---

## Q1 — Who are you and what are your main domains?

Briefly describe yourself and your active life domains, mapped to your confirmed buckets. Two or three sentences per domain — what it is, your role in it, and roughly how much time and energy it demands.

```
[Your answer here]
```

---

## Q2 — Paste 1–2 things you've written recently. Don't edit them.

An email, a message, a doc, a post — anything that sounds like you when you're not performing. **Paste verbatim from real prior writing.** Do not type these fresh mid-conversation — samples written during onboarding are already shaped by that context, making them less useful as voice references.

```
Sample 1:
[paste here]
```

```
Sample 2 (optional but recommended):
[paste here]
```

---

## Q3 — What are your biggest priorities for the next 90 days?

List 2–4 things that, if not done by then, would make you feel like the quarter was wasted. Be specific — not "grow the business" but "close 10 paying customers by June 30." Label each with its bucket.

```
1. [Priority] ([bucket])
2. [Priority] ([bucket])
3. [Priority — optional] ([bucket])
4. [Priority — optional] ([bucket])
```

---

## Q4 — Who are the key people in each of your domains?

First name and role — last name only if needed for disambiguation. Aim for 3–5 per bucket. Just the people who come up most often.

```
[Bucket 1 name]:
- [Name] ([role or relationship])

[Bucket 2 name]:
- [Name] ([role or relationship])

[Bucket 3 name — if applicable]:
- [Name] ([role or relationship])
```

---

## Q5 — Where do you communicate day-to-day?

Email accounts and clients, messaging apps (Slack, Teams, Discord, iMessage), customer or team platforms. Break it down by bucket if the tools differ significantly. Include your calendar setup.

```
[Your answer here]
```

---

## Q6 — Where does information live, and what AI tools do you use?

**Part A — Information landscape:** Notes, recordings, documents, reference files, knowledge bases. Include everything, even if it's scattered or a mess.

**Part B — AI stack:** Tools and subscriptions you currently use or pay for. Include anything active, even occasionally.

```
Part A:
[Your answer here]

Part B:
[Your answer here]
```

---

## Q7 — Where do tasks and projects actually live?

The system you actually use — not the ideal one. Task managers, project tracking tools, and whether different domains use different systems. "Mostly in my head" is a valid answer.

```
[Your answer here]
```

---

## Q8 — What's your biggest recurring friction?

The one thing that reliably drains time or creates drag each week. Be specific.

```
top_friction:
[Your answer here]
```

---

## Q9 — Where does money come in and go out?

Simple finances (one salary, personal spending only): one line is fine. If you have business income, multiple revenue streams, or financial complexity across buckets, go a bit deeper. Cover income sources, payment tools, and how you currently track expenses.

```
[Your answer here]
```

---

When this file is complete, run `/onboard` (or re-run it) and the skill will scaffold your Day-1 file set: root `context/` files, bucket-level `context/` files, a populated `connections.md`, and a filled `CLAUDE.md`.
