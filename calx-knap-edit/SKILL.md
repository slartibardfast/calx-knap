---
name: calx-knap-edit
description: Rewrite, author, lint, or expand a single text in the calx-knap controlled register, interactively. Use whenever the user says "knap this", asks to rewrite a file, paragraph, or prompt in calx-knap or "the register", wants a new prompt, memory, or skill file drafted directly in the register, asks to check text against the K-rules, or wants a knapped file expanded back to longhand. For whole-corpus compression with model-gated acceptance, use calx-knap-corpus; this skill drafts and lints only and never accepts.
---

# calx-knap edit

Spec: `calx-knap.md`, repo root. Read BEFORE first edit. IF absent, STOP.

Scope: one text per request. Output is DRAFT, ungated. NEVER write `build/` or the ledger.

`.calx-knap/wordlist.md` present: obey it; append new terms; flag K1 and K2 collisions.

Spec ambiguity: report in draft notes; IF `.calx-knap/spec-issues.md` exists: append there. NEVER improvise a rule.

## Knap

1. Lift byte zones: fences, inline code, paths, URLs, error strings, numbers with units. Substitute `[[Zn]]`.
2. Rewrite per the K-rules. One rule per line. Source headings named by numeral or position: rename to content (K17).
3. Gate. IF `.calx-knap/overrides.md` binds `gate`: run the command. Exit 0 REQUIRED. Exit 3: advisory; review, rewrite or proceed. Exit 2: STOP, report. ELSE hand-check: zone diff against source empty; every source negation, exception, bound present; 12-token budget, 20 cap, on the zone-lifted line; ASCII surface (K16).
4. Reinsert zones. Return draft with notes: token delta per target tokenizer; elisions applied; lines kept verbatim, with reason.
5. K6 doubt: keep the word; mark the line `?`.

## Author

- Collect content longhand first; user confirms meaning; THEN knap. NEVER invent rules while compressing.
- Blocks: six lines max, topic-labelled (K13).

## Expand

- Knapped file in, longhand gloss out: `<name>.gloss.md`, line-keyed (K14).
- Expansion adds words, NEVER content. Ambiguity met while expanding: report it; the source line is suspect.

## Check

- On "check against K-rules": violations by K-number and line. No rewrite UNLESS asked.

## Hand-off

- Output bound for deployment: say once; gate via calx-knap-corpus BEFORE acceptance.
