# calx-knap loop v1

Context files: `calx-knap.md` (SPEC), `.calx-knap/wordlist.md` (WORDLIST), `.calx-knap/ledger.tsv` (LEDGER), `.calx-knap/overrides.md` (BINDINGS).
Compressor: the invoking model. Targets: the canary and family models named in BINDINGS.
One doc per iteration. Mode: IF message has a RESULTS block, run ADJUDICATE. IF NOT, run DRAFT.

## DRAFT

Input: one SOURCE doc + SPEC + WORDLIST + LEDGER.

1. Rules. List each normative statement in SOURCE. Tag class:
   NEG (negation, exception), LIM (numeric limit), PRE (precedence), TRG (conditional trigger), PLAIN.
2. Probes. Write BEFORE compression, from SOURCE only.
   - Probe text: full longhand English, NEVER the register. The doc under test is the only compressed variable.
   - One probe per rule. Two per NEG, LIM, PRE, TRG.
   - Probe = `{id, class, input, expected, check}`. check MUST be mechanical: exact string, regex, option pick. Free-text check only when unavoidable; the BINDINGS judge model scores those, NEVER you.
   - NEG probes cover base path AND exception path.
   - Mark two probes CTX: rerun at 60% context fill.
   - Freeze. NEVER edit a probe after this step, UNLESS it contradicts SOURCE; then cite the SOURCE line.
3. Byte zones. Inventory spans: code, commands, paths, URLs, numbers with units, error strings.
4. Compress per the K-rules. Source headings named by numeral or position: rename to content (K17). New terms: append to WORDLIST; flag K1 and K2 collisions.
5. Gate, mechanical. IF BINDINGS name a `gate` command: run it (contract: `gates.md`). Exit 0 REQUIRED before probes. Exit 3: advisory; review, rewrite or proceed. Exit 2: STOP, report. Gate absent: check the K-rules by hand; ledger notes `unlinted`.
6. Emit:
   - compressed doc, one instruction per line (K5)
   - `probes.jsonl`
   - run plan: each probe, original AND compressed, on ALL targets, repeats per BINDINGS, majority vote; CTX probes also at fill
   - token counts per BINDINGS tokenizer
7. Ledger: append `{doc, drafted, tok_src, tok_knap, reduction}`.

STOP. Await RESULTS.

## ADJUDICATE

Input: RESULTS per probe, per model, per arm, per repeat.

Gates, in order; each named by its passing condition:

- structural-clean: the bound gate exits 0 on the compressed doc. Unbound: hand-checked; ledger notes `unlinted`.
- zone-diff-empty: byte-zone diff against SOURCE empty.
- canary-parity: the canary on compressed matches original on ALL probes.
- no-regression: no NEG, LIM, PRE, TRG regression on ANY model. Family agreement counts as one vote beside the canary.
- cosine-floor: cosine(SOURCE, compressed) >= 0.91, per the BINDINGS embedder.
- yield-floor: reduction >= 25%. Below: keep original, status LOW-YIELD. Not a defect.

Verdicts:

- ALL pass: ACCEPT. Ledger: accepted, scores, reduction.
- Gate fail: REVISE. Name failing probes, locate the line, minimal diff, rerun changed lines only. Max 2 revisions.
- Third fail: REVERT. Ledger: kept-original, ids of failed probes.

Failures fix the document, NEVER the probe (DRAFT, probes step).
After verdict: next pending doc, run DRAFT.

## Invariants

- SOURCE stays canonical (K14). Compressed files are build artifacts.
- Harness text (probes, prompt templates, grader instructions) is longhand English, exempt from the K-rules (SPEC, scope boundary).
- WORDLIST append-only during the pass. Freeze at corpus end; then one reconciliation pass for K1 collisions.
- BINDINGS name what rots; what BINDINGS may and may never touch: `gates.md`.
- SPEC ambiguity: log to `.calx-knap/spec-issues.md`. Do NOT improvise a rule mid-pass.
- Report per iteration fits one screen: verdict, gates, reduction, open issues.
