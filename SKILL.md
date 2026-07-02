---
name: atomic-readiness
description: Decomposes a product into atomic, independently evaluable units with per-lane readiness rubrics and hard evals, then drives the improvement loop — scoring lanes, ranking gaps, and generating dispatch-ready atom briefs for parallel agents. Use when starting an eval-driven improvement program, breaking a project into improvement lanes, asking "what should we improve next", or bootstrapping readiness/maturity tracking for a codebase.
when_to_use: Also use when auditing whether a project's evals can be trusted (self-grading honesty), when setting up parallel agent lanes that must not collide, or when a "make X self-improving" directive lands.
version: 0.1.0
metadata:
  tags: [readiness, evals, decomposition, improvement-loop, agents, grading]
---

# Atomic Readiness

Turn any product into a system that improves itself: journey-ordered **lanes**,
atomic **units** with explicit seams, per-lane **readiness rubrics** scored by
honest producers, a **hard-eval harness**, and a **drive loop** that always knows
the next most valuable atom and emits a brief an agent can execute in isolation.

Proven end to end on Iris (reference implementation: `aneym/iris` —
`docs/reports/improvement-map/`, `docs/reference/readiness-scoring.md`,
`docs/reference/trusted-self-grading.md`).

## Mode selection

- **First run on a project** (no `improvement-map/` or equivalent exists) →
  **BOOTSTRAP** (phases A–C below).
- **Already bootstrapped** (rubrics + units exist) → **DRIVE** (phase D).
  Invoking this skill on a bootstrapped project means: score, rank gaps,
  emit the next atom briefs.

## Non-negotiables (the soul — apply in every phase)

1. **Builders never self-certify.** A unit is done when a different executor
   re-runs its proof. Every claim ships with its rerunnable proof command.
2. **Producers read real substrate only.** A readiness criterion greens only
   when the thing it measures already exists and the producer reads it.
   `producer: unbuilt` (an honest red) always beats a fabricated green.
   Readiness never grows a private test harness to satisfy itself.
3. **Honest classification.** Infra absence is an infra-skip with a visible
   reason — never a fail, never a fake pass. A genuine defect still fails loudly.
4. **Measurement may correct itself, never game itself.** Rubric changes are
   versioned. A category correction (criterion measured the wrong kind of thing)
   is legitimate; threshold-thinning to force green is not. Say which one you did.
5. **Labels vs machinery.** Judgment labels (goldens, relevance grades, quality
   scores) come from the highest-judgment agent available. Machinery (runners,
   schemas, producers) is delegable. Never let a lane invent judgment labels.
6. **Deterministic ground truth first.** Prefer criteria derivable from rules,
   schemas, and data over model-judged ones. Where labels are needed, freeze them.
7. **One owner per seam.** Parallel lanes never share files. Shared-structure
   changes (schema, routes, exports) get one owner per wave.

## BOOTSTRAP

Copy this checklist and check off items as you complete them:

- [ ] A1: Map the user journey; order lanes by it (upstream quality gates everything downstream)
- [ ] A2: Decompose lanes into atomic units with explicit file-path seams → `units.tsv`
- [ ] A3: Land join tables as checked-in DATA (unit↔lane, feature↔lane) — never code constants
- [ ] B1: Author one rubric per lane (criteria + anchors + thresholds + maturity ladder)
- [ ] B2: Bind producers only where substrate exists; mark the rest `producer: unbuilt`
- [ ] B3: Validate the rubric mechanically (schema check) and score once — reds are the backlog, not a problem
- [ ] C1: Stand up the harness conventions: RED-first atoms, runs ledger, isolated sandboxes
- [ ] C2: Add flow-level evals (user stories) with honest predicates + infra-skip classification
- [ ] C3: Wire independent re-verification (different executor, out-of-repo baseline)

Read the phase file before executing it (one level deep, read on demand):

| Phase | File | Contents |
|---|---|---|
| A — Decompose | `reference/decompose.md` | Lane/unit/seam rules, units.tsv columns, join tables |
| B — Measure | `reference/rubrics.md` | Criteria authoring, producer types, rubric schema, maturity ladder |
| C — Harden | `reference/harness.md` | Eval-first atoms, runs-ledger rows, sandboxes, stories, re-verification |

Templates to copy: `templates/rubric.example.json`, `templates/units.example.tsv`,
`templates/atom-brief.md`.

## DRIVE

Copy this checklist each drive cycle:

- [ ] D1: Score every lane; record a snapshot (producer-tagged, timestamped)
- [ ] D2: Diff vs the last snapshot; a regression files an issue before anything else
- [ ] D3: Rank the gap queue (journey order × maturity gate × criterion weight)
- [ ] D4: For the top gaps, emit dispatch-ready atom briefs (template) — seam-disjoint set
- [ ] D5: After atoms land: re-score, show the Δ, update the lane's history page
- [ ] D6: Fold durable learnings into the project's knowledge base (wiki page per landmine)

Read `reference/drive.md` for ranking rules, the brief format contract, the
merge-train verification pattern (batch tsc/lint once per wave, not per atom),
and the always-running-priority-lane rule.

Feedback loop: only proceed past D1 when the rubric validates; only call a cycle
done when the Δ table exists and every dispatched brief has a seam owner.

## Quick search

Locate concepts across the skill without full reads:

```bash
grep -rn "<keyword>" "${CLAUDE_SKILL_DIR}/reference/" "${CLAUDE_SKILL_DIR}/templates/"
```

Common keys: `seam`, `producer`, `unbuilt`, `infra-skip`, `runs-ledger`,
`atom brief`, `maturity`, `regression`, `goldens`, `merge train`.
