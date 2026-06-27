---
name: skill-refine
description: Targeted fixes for installed skills with a known specific problem. Use when a skill is misfiring (triggers too often, not enough, or at the wrong time), producing output that is the wrong format, length, or quality, or doing too much or too little. Trigger on: "my skill isn't working right", "fix my skill", "this skill keeps firing when it shouldn't", "my skill won't trigger", "the output from my skill is wrong", "this skill needs tweaking", "a skill I use is broken", "refine this skill", "my skill is overtriggering", "my skill is undertriggering", or any description of a specific known problem with an installed skill. Do NOT use for creating new skills from scratch or running formal evals — route those to /skill-creator instead.
---

## What this skill does

Diagnoses a specific known problem in an installed skill and applies a targeted fix. One session = one diagnosed issue + one set of changes written to the skill file.

**What it is:** A fast, conversational repair loop. You describe what's wrong, this skill reads the current skill, identifies the root cause, shows you exactly what would change, and writes the fix after you confirm.

**What it is not:** A full rebuild or eval session. If the skill needs to be rethought from the ground up, or if you want formal test cases and benchmarks, use `/skill-creator` instead. If you're not sure which one you need: targeted fix for a known problem → here. General improvement or starting over → skill-creator.

---

## The three issue types

Every skill problem falls into one of these:

**Type A — Trigger problem**
The skill fires when it shouldn't, doesn't fire when it should, or fires inconsistently. The fix lives in the `description` frontmatter — the primary mechanism that determines when Claude consults the skill.

**Type B — Output quality problem**
The skill triggers correctly but the result is wrong: wrong format, wrong length, missing elements, wrong tone, or unwanted extras. The fix lives in the execution section of the skill body.

**Type C — Scope problem**
The skill is doing the wrong thing, doing too much, or stopping too early. Narrow scope drift → targeted fix in the body. Fundamental mismatch → route to `/skill-creator`.

---

## Execution

### Step 1: Identify the skill

Ask: "Which skill are we looking at, and what's it doing wrong?"

Read the skill from `.agents/skills/[skill-name]/SKILL.md`. Confirm you have the right one before proceeding.

If the user is not sure which skill is causing the problem: ask them to describe the phrase they used and what happened. Match against installed skills by reading frontmatter descriptions in `.agents/skills/`.

### Step 2: Diagnose the issue type

Ask two focused questions — no more:

1. "What is the skill doing that it shouldn't, or not doing that it should?"
2. "Can you give me a specific example — what did you say or do, and what happened?"

From these two answers, determine the issue type. If genuinely unclear, ask one follow-up. Do not run a long interview.

**Trigger signals:**
- "It fired when I said X and I didn't mean to use it" → overtriggering
- "I said the exact phrase and nothing happened" → undertriggering
- "It works sometimes but not always" → description is too vague or too dependent on exact phrasing

**Output signals:**
- "It ran but the result was [too long / wrong format / missing something / included something I didn't want]" → Type B
- Ask for an example of what they got vs. what they expected

**Scope signals:**
- "It does X but keeps going and does Y too, which I don't want" → doing too much
- "It stops after X but should continue to Z" → doing too little
- "It does something completely different from what I expected" → possible fundamental mismatch; assess for skill-creator routing

### Step 3: Propose the fix

**For Type A — Trigger fixes**

Read the current `description` field carefully. Identify the cause:

- *Overtriggering*: description is too broad or lacks exclusion conditions. Add specific exclusion language ("Do NOT use when...") or narrow the trigger phrases.
- *Undertriggering*: description relies on exact phrases users don't naturally say, or lacks enough context cues. Add natural-language variants, situational triggers, or make it more explicit about when to fire. Per skill-creator guidance: descriptions should lean toward being specific about when to trigger — Claude tends to undertrigger.
- *Inconsistent*: description mixes specific and vague triggers. Tighten the specific ones and remove the vague ones.

Show the proposed change as a before/after for the description field only:

```
BEFORE (description):
[current description text]

AFTER (description):
[proposed description text]
```

**For Type B — Output quality fixes**

Read the execution section. Identify the specific instruction producing the wrong behavior. Change only those lines — do not rewrite the whole section.

Show before/after for the affected lines only, with approximate line number:

```
BEFORE (around line N):
[current instruction]

AFTER:
[proposed instruction]
```

If the problem is a missing instruction (the skill simply never had a rule for this), add it to the appropriate section and show where it fits.

**For Type C — Scope fixes**

*Narrow drift* (a little too much or too little): find the instruction that defines where the skill starts or stops and tighten it. Show before/after.

*Fundamental mismatch* (the skill is doing the wrong thing entirely): do not attempt a patch. Say:

> "This looks like a more fundamental issue than a targeted fix can address — the skill's core logic may need to be rethought. I'd recommend taking this to `/skill-creator` with the current skill as the starting point and running a proper iteration loop there."

Then stop. Log the diagnosis to `decisions.md` and close the session.

### Step 4: Confirm before writing

Show the full before/after and ask: "This is what will change. Ready to apply?"

Do not write anything to the file until you have explicit confirmation.

### Step 5: Write the fix

Write the updated content to `.agents/skills/[skill-name]/SKILL.md`.

If the change touches more than a few lines, offer to back up the original first:
"Want me to save the current version to `archives/skill-backup-[skill-name]-[date].md` before I overwrite?"

### Step 6: Sanity check

After writing, confirm the fix addresses the specific problem:

> "With this change, [the exact scenario the user described] should now [the desired behavior]. Does that match what you expected?"

If the user wants to verify: "Try triggering it with [the phrase that was misfiring] and see if the behavior has changed."

### Step 7: Log the change

Append to `decisions.md`:

```
## [date] — skill-refine: [skill-name]
Issue type: [Trigger / Output / Scope]
Problem: [one sentence]
Fix: [one sentence on what changed]
```

---

## When to route to skill-creator

Route there — and stop — when any of these are true:

- The scope is fundamentally wrong and fixing it would require rewriting major sections
- The user wants formal test cases, quantitative benchmarks, or description optimization via script
- The skill needs to be reimagined, not tweaked
- The user says something like "I basically want a different skill" or "let's start over"

Say: "This is more of a rebuild than a targeted fix. `/skill-creator` has a full iteration and eval loop better suited for this — take the current skill there as a starting point."

---

## Notes

- **Targeted only.** Change the minimum needed to fix the stated problem. Do not refactor or improve other parts of the skill while you are in there.
- **Before/after for changed lines only.** Not the full skill. The user needs to see exactly what is different without reading the whole file.
- **One issue per session.** If a second issue surfaces mid-session, note it and offer a second pass after the first fix is confirmed — do not try to fix multiple independent issues at once.
- **Dynamic skill path.** Skills live in `.agents/skills/`. Read the directory to find what is actually installed — never assume skill names.
- **Description optimization at scale.** For trigger fixes, the conversational approach here covers most cases well. If the user wants formal verification (running multiple test queries and scoring trigger rates across iterations), `/skill-creator` has a script for that.
- **After any skill change**: if the fix affects the skill's name, trigger description, or primary function, update the corresponding row in `HANDBOOK.md` — it is the single source of truth for the installed skill inventory.
