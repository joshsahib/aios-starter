---
name: grill-with-context
description: Deep-interview the user about any plan, design, or decision while actively building shared vocabulary and documenting non-obvious decisions. Reads existing context first, maps the session, then works through grouped batches of 1-5 related decisions per turn — each with options and a recommendation. Writes session capture to threads/, appends Decision Records to decisions.md. Use when stress-testing a plan, extracting thinking into a lasting doc, or when the user says "grill me".
---

# Grill With Context

Interview the user about a topic until you reach shared understanding — grouping related decisions into efficient batches, actively refining shared language, and capturing what's decided so it survives context resets.

**Two outputs, two destinations:**
- Session capture (batched Q&A log, vocabulary, open flags) → `threads/`
- Decision Records → `decisions.md` (appended after each batch, not deferred to the end)

Your role is not to confirm — it is to **stress-test**. Surface risks. Challenge assumptions. If the direction looks wrong, say so. Presenting options with a recommendation is an efficiency tool, not an endorsement; argue against your own recommendation if there's a better path.

---

## Before the first batch

1. **Scan existing context.** Check for `SOUL.md`, `CLAUDE.md`, `AGENTS.md`, and the current bucket's `context/` subfolder. Read relevant files. Extract what's already known — don't ask what you can read. Only surface what's net-new or genuinely unclear.

2. **Load the shared vocabulary.** If a `context/` folder exists, read any glossary or domain files. You'll challenge user language against these during the session and propose updates at the end.

3. **Locate `decisions.md`.** Find it at root or nearest parent. Read recent entries to avoid relitigating settled decisions. Note its path — you'll append to it mid-session.

4. **Create the session capture file** at `threads/{YYYY-MM-DD}-{topic-slug}.md` relative to the current working directory. Get today's date with `date +%F`.

5. **Initialize the capture file** with: title, date, goal, and empty sections for Shared Vocabulary, Q&A Log, and Open Flags. Decision Records go to `decisions.md`, not here.

6. **Map the session.** Before asking anything, identify the major decision areas and rough batch count. Present this briefly to the user — one line per area, total batch estimate. This lets them redirect if something is already settled or out of scope.

7. **Tell the user where you're saving** (capture file + decisions.md) in one line. Then begin Batch 1.

**Session map format:**
```
Saving to: threads/{date}-{slug}.md · decisions.md

Session map — {topic}
~{N} batches covering:
① {Area name} — {one phrase}
② {Area name} — {one phrase}
③ {Area name} — {one phrase}
...

Starting with Batch 1.
```

---

## Batch design

Group decisions by **shared dependency level and domain**. A good batch is one where the decisions don't strongly depend on each other, or where dependencies flow top-to-bottom within the batch (A → B → C, not circular).

**Batch sizing by decision weight:**
- **1–2 per batch:** Foundational or architectural decisions with large downstream consequences. Give each room.
- **3–4 per batch:** Related medium-weight decisions that share a context but are independently answerable.
- **5 per batch:** Lighter confirmations, sequencing choices, or assumptions that can be scanned quickly.

If an answer to one decision in a batch would clearly invalidate another in the same batch, move the dependent decision to the next batch or mark it as contingent.

---

## Batch format

Each batch turn follows this structure:

```
---
Batch {N} of {total} — {Category name}

{One sentence of context if the domain is non-obvious.}

**1. {Decision name}**
- **Option A (recommended):** description — why preferred
- **Option B:** description — trade-off
- **Option C:** description — trade-off

**2. {Decision name}**
- **Option A:** description — trade-off
- **Option B (recommended):** description — why preferred

...

{Vocabulary note if a term needs flagging mid-batch.}
---
```

Keep option descriptions tight — one line each. The reasoning for the recommendation should be direct, not exhaustive. If a decision only has one real answer given the context, say so and state why rather than manufacturing false alternatives.

---

## Checkpoint rule

After the user responds to a batch — before presenting the next one:

1. Append a structured entry per decision to the capture file (see structure below).
2. Update the Shared Vocabulary section for any new or refined terms.
3. Append qualifying Decision Records to `decisions.md` now — not at session end.
4. Reconcile any intra-batch dependencies (if answer to #1 changes #3, update the captured entry).
5. Then present the next batch.

The file, not your context, is the source of truth. Don't let more than one batch accumulate without checkpointing.

---

## Decision Records — what qualifies and where they go

Record a decision only when it is:
- **Hard to reverse** — a structural commitment, not an easily-changed default
- **Surprising without context** — a future reader would ask "why?"
- **The product of real trade-offs** — not the obvious choice

**Format — append to `decisions.md`:**

```markdown
## YYYY-MM-DD — Short title
**Session:** grill-with-context · {topic-slug}
**Decision:** what was decided.
**Why:** the reasoning, constraints, and what would change your mind.
**Alternatives considered:** what else was on the table.
**Owner:** [who's accountable]
```

Keep it terse. The **Why** should capture constraints and what would change the decision — that's what makes it useful later. Don't duplicate Decision Records in the capture file; the Q&A log covers session context.

---

## Language refinement

Treat imprecise language as a problem to solve, not a note to log.

- **Challenge fuzzy terms inline.** If a term is vague, flag it within the batch response rather than blocking the batch: *"I'm treating 'X' as [definition] here — correct me if that's wrong."* If it needs real resolution, pull it into its own batch item.
- **Spot terminology collisions.** If the user's language conflicts with existing vocabulary or prior usage, surface the collision explicitly with two or three resolution options.
- **Capture agreed terms immediately** in the Shared Vocabulary section.
- **At session end**, offer to update the bucket's `context/` files with any new additions.

---

## Capture file structure

```markdown
# {Topic}: Thread Capture / Discovery Notes
Date: {date} · Goal: {one line}
Decisions log: {path} (appended directly — not duplicated here)

## Shared Vocabulary
| Term | Definition | Notes |
|------|------------|-------|

## Summary / key decisions
(running synthesis — updated each checkpoint)

## Q&A log

### Batch 1 — {category}

**1. {decision name}**
- Captured: {what was decided or confirmed, in their words where it matters}
- Vocabulary: {any term established}
- Flags: {open item → owner}

**2. {decision name}**
- Captured: ...

### Batch 2 — {category}
...

## Open flags
- {item} → {owner}
```

---

## At the end

1. Read the capture file for contradictions, gaps, and decisions that depend on open flags. Reconcile what you can.
2. Confirm all qualifying Decision Records were written to `decisions.md`.
3. Offer to update the bucket's `context/` files if meaningful vocabulary was established.
4. Give a short recap: batches covered, flags open, vocabulary added, decisions logged, suggested next step.
