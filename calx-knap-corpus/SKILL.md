---
name: calx-knap-corpus
description: Apply the calx-knap controlled register to a corpus of prompt, memory, and skill files, then gate every compressed file behind pre-registered behavioral probes on a fixed model spread before acceptance. Use this whenever the user says "knap the corpus", "calx-knap", "run the knap gate", "compress the corpus", asks to shrink a set of prompt or memory or CLAUDE.md files, or wants compressed prompts verified against models, including re-verification after source files change. Originals stay canonical; compressed files are build artifacts accepted one document at a time. For one-off interactive rewrites, authoring, or expansion without gating, use calx-knap-edit instead.
---

# calx-knap corpus scheduler

Spec: `calx-knap.md`, repo root. Protocol: `references/loop.md`, DRAFT and ADJUDICATE, per document. Gate contract and bindings format: `references/gates.md`. Read all three BEFORE first document. IF any absent, STOP.

Single-text rewrites, authoring, expansion: calx-knap-edit skill. This skill alone writes `build/` and the ledger.

## Layout

```
src/              originals; canonical; NEVER edit
build/            artifacts, same relative paths; ships complete
.calx-knap/       loop state, one namespace:
  probes/         probes.jsonl per document; frozen at DRAFT
  overrides.md    per-repo bindings; format in references/gates.md
  wordlist.md     project word list (K1, K2); append-only during pass
  ledger.tsv      per document: status, tokens, scores
  spec-issues.md  spec ambiguities; NEVER improvise a rule
```

## Bindings

`.calx-knap/overrides.md` names the model spread, endpoints, gate command, judge, embedder, tokenizers. Bindings rot: confirm BEFORE first run. IF the file is absent: collect bindings from the user; write the file; THEN start. IF `.calx-knap/wordlist.md` is absent at first document: create empty (terms accrue during the pass). Canary: the weakest deployed model (a rule that holds on the canary holds upward; SPEC, decoder budget). Family models share lineage; family agreement counts once beside the canary.

## Schedule

1. Order pending documents smallest first (fast feedback).
2. Per document: loop DRAFT; await RESULTS; loop ADJUDICATE.
3. Hash source at verdict. Changed hash reenters queue; unchanged skips.
4. ACCEPT writes the compressed artifact. REVERT copies source verbatim; build stays deployable whole.
5. Corpus end: freeze wordlist; one K1 reconciliation pass; report accepted, reverted, low-yield, tokens before and after, failures by class.

## Gate

Contract: `references/gates.md`. IF `.calx-knap/overrides.md` binds `gate`: exit 0 REQUIRED before probes and at the structural-clean gate. Gate absent: hand-check the K-rules; ledger notes `unlinted`. Outer prose linters measure human-audience prose, NEVER the corpus; an adopting repo scopes them with that tool's own configuration (gates.md, scope).
