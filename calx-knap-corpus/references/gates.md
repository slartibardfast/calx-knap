# gates

The contract between the calx-knap loop and the measuring tools of an adopting repo. No tool is named here: the repo binds its own in `.calx-knap/overrides.md`, and the loop treats every binding through this contract.

## Exit contract

The structural gate is any command that checks a compressed artifact against the K-rules and exits:

```
0  clean       REQUIRED before probes and at the structural-clean gate
1  refuted     a K-rule violation with a located line; fix the document
2  unusable    missing input (source, wordlist, zones, tokenizer); STOP, report
3  advisory    review each finding; rewrite, or proceed with reason
```

## Structural checks

A conforming gate checks, per artifact:

- every line parses against the SPEC ABNF; operator positions hold.
- terms against `.calx-knap/wordlist.md`; synonym rotation (K1, K2).
- token budget per BINDINGS (default 12, cap 20), per target tokenizer, gate on the max, measured on the zone-lifted line (K5).
- every SOURCE negation, exception, numeric bound present as operator or bound (K6, K8).
- no pronouns across lines (K7); one negation per line, BEFORE its verb (K9).
- zone reinsertion diff against SOURCE empty (K12); six lines max per block (K13).
- ASCII surface on register lines (K16); content-named labels (K17).

Diagnostics: `<file>:<line> K<n>: <message>`, deterministic order.

## Bindings: .calx-knap/overrides.md

Per-repo bindings, one per line, `key value`, `#` comments. The file is the single place a deployment names things that rot.

```
# example bindings
gate       <command implementing the exit contract>
canary     <weakest deployed model> <endpoint>
family     <model, model, model>
judge      <family model that scores free-text checks>
embedder   <embedding model for cosine-floor>
repeats    3
tokenizers <tokenizer, tokenizer>
budget     12 20
```

MAY be bound: gate command, canary, family, endpoints, judge, embedder, repeats, tokenizers, K5 numeric budgets.
NEVER bound: the operator set (K8), probe freeze (loop, probes step), SOURCE canonicality (K14), failures-fix-the-document (loop, verdicts).

File absent: the corpus skill collects bindings first; the edit skill hand-checks and marks drafts `unlinted`.

## Scope

```
repo prose (spec, README, records)    human-audience; an outer prose linter measures it
src/ sources, .calx-knap/ loop state  longhand; ungated here
build/ artifacts, register skills     machine-audience; the structural gate alone measures them
```

The boundary is audience. Register text is shaped like the devices outer anti-slop linters detect (fragments, parallel lines, capitalized keywords); that is surface, not slop, and an anti-slop ruler mis-measures a machine register. An adopting repo that audits tracked docs with an outer prose linter scopes it using that tool's own exclusion mechanism; nothing in the register names or configures the tool. Longhand `src/` originals are ordinary prose and MAY carry the marks such linters flag: exclude `src/` there too, or accept advisory findings.
