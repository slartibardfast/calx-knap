# calx-knap

A controlled register for prompt and memory files. *Knap*: to shape flint by striking flakes off to leave the working edge. *Calx*: the counting pebble that gave us *calculus*; in the alchemists' books, the residue that survives calcination. The register removes mass and keeps the edge.

The design in one sentence: position does the work of ballast function words, a fixed operator lexicon does the work of logic, and nothing else is grammar. [calx-knap.md](calx-knap.md) is the specification: the design sources, the operator class, the numbered K-rules, a worked example, and the decoder-budget limits.

This page is the operator manual: what you do, at which step, when an agent runs the skills. The agent's own procedure lives in the skill files and in [calx-knap-corpus/references/loop.md](calx-knap-corpus/references/loop.md); nothing there is repeated here.

## Install

Copy or link the two skill directories into your agent's skills folder (for Claude Code, `.claude/skills/`). Copy `calx-knap.md` to the root of the repo the skills will operate on: both skills read the spec there and stop without it.

## Rewrite one text

Say "knap this", or ask for a check against the K-rules, or ask for a knapped file to be expanded back to longhand. The [calx-knap-edit](calx-knap-edit/SKILL.md) skill fires. Everything it produces is a draft: it never writes `build/`, and deployment always goes through the corpus loop.

## Compress a corpus

The [calx-knap-corpus](calx-knap-corpus/SKILL.md) skill compresses a set of documents one at a time and accepts nothing without measurement. Your seat in the loop:

1. Put the originals under `src/`. They stay canonical; the loop never edits them.
2. Say "knap the corpus". On first run the agent asks for your bindings and writes `.calx-knap/overrides.md`: which canary and family models to test against, their endpoints, the judge, the embedder, repeats, tokenizers, and the structural gate command if you have one. Without a gate command the agent checks the K-rules by hand and the ledger marks each draft `unlinted`.
3. For each document the agent drafts: behavioral probes written first and frozen, then the compressed draft, then a run plan.
4. You execute the run plan: each probe against the original and against the compressed document, on every target model, with the stated repeats. Reply with a RESULTS block, one outcome per probe, model, arm, and repeat. Running the plan is manual today; a runner script can take it over later.
5. The agent adjudicates against the gates in loop.md and records the verdict in the ledger. A failing document gets at most two revisions; the third failure reverts it to the original.
6. Repeat per document. A source that changes after its verdict re-enters the queue by hash; unchanged sources skip.

## What you review

Some outcomes are yours to judge, and the agent is built to wait for you:

- An advisory gate exit (exit 3) lets the agent rewrite or proceed with a stated reason. Skim the reasons.
- LOW-YIELD means the compression saved under 25% and the original was kept. It records a fact about the document, never a defect.
- `.calx-knap/spec-issues.md` collects rule ambiguities the agent met mid-pass. The agent must never improvise a rule; you rule on each entry, and accepted rulings become spec changes.
- At corpus end the word list freezes and the agent reports K1 collisions for one reconciliation pass, beside the acceptance and token totals.

## The files

The loop keeps its state under one namespace, `.calx-knap/`, at the root of the repo it operates on. The corpus itself stays visible: sources in `src/`, artifacts in `build/`.

- `.calx-knap/overrides.md` is yours: every binding that rots with time, in one place. Format in [calx-knap-corpus/references/gates.md](calx-knap-corpus/references/gates.md).
- `.calx-knap/wordlist.md` is the project word list, append-only while a pass runs.
- `.calx-knap/ledger.tsv` is the per-document record: status, tokens, scores.
- `.calx-knap/spec-issues.md` is the ambiguity queue described above.
- `.calx-knap/probes/` holds the frozen probes, one file per document.
- `build/` holds accepted artifacts at the same relative paths as `src/`, deployable as a whole. Never edit it by hand.
