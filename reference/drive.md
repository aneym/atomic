# Phase D — Drive: score, rank, brief, loop

## Contents
1. Scoring and snapshots
2. Regression tripwire
3. Ranking the gap queue
4. Atom briefs (the dispatch contract)
5. Waves, seams, and the merge train
6. Standing rules
7. Loop feedback (the system corrects itself)

## 1. Scoring and snapshots

Each drive cycle scores every lane (all producers run), writes a
producer-tagged snapshot (who scored, when, sha), and renders the per-lane
table: pct, maturity level, north-star status. Snapshots are append-only.

## 2. Regression tripwire

Before dispatching anything new: diff vs the previous snapshot. Any criterion
that flipped green→red files an issue (project intake) immediately — a
regression outranks all new work. Silence past a promise is the one sin.

## 3. Ranking the gap queue

Rank failing/unbuilt criteria by:
1. **Journey order** — upstream lanes first (bad supply poisons everything).
2. **Maturity gate** — criteria holding a lane at a lower level beat
   same-level extras (unblock the ladder).
3. **Determinism** — buildable-offline atoms before ones needing live deps.
4. **Weight** — the rubric's own weights break ties.

Split every gap into **machinery** (delegable build) vs **labels** (judgment —
reserved to the highest agent). A gap whose criterion is unsatisfiable as
written is loop feedback, not a build atom (see §7).

## 4. Atom briefs (the dispatch contract)

Emit one brief per top gap using `templates/atom-brief.md`. A brief is
dispatch-ready when a fresh agent could execute it without asking anything:
concrete behavior change, exact seam paths (owned + forbidden), the eval
command, RED-first requirement, done-condition, and the reporting envelope
(parsed pass/fail counts + what remains unproven — never "done" on say-so).

## 5. Waves, seams, and the merge train

- Dispatch seam-disjoint briefs in parallel; anything sharing a file
  serializes into one lane.
- Builders open PRs and never merge. The auditor reruns focused suites from
  the builder's workspace, parses fail counts (never gates on grep/pipe
  success), and merges on green.
- Batch per wave: one typecheck, one lint, one deploy-sync per merge train —
  not per atom. Generated/derived tracked files (API typedefs) regenerate and
  land as a train step.
- A red train freezes merges: fix forward before anything else lands.

## 6. Standing rules

- The highest-priority lane ALWAYS has an active improvement atom; if its gap
  queue empties, generate the next criteria wave before dispatching elsewhere.
- Every merged lane that produced a durable learning adds/updates a knowledge
  base (wiki) page in the same PR.
- Lane-tag the project timeline so "everything that happened to lane X" is one
  filter.
- Human-in-the-loop is targeted: when a lane needs human validation, generate
  a guided run (steps + deep link + per-step feedback capture), don't ask for
  a general QA pass.

## 7. Loop feedback (the system corrects itself)

Expected, healthy events — handle them explicitly:
- **Unsatisfiable criterion** → category correction (versioned) + note what
  the data model would need.
- **Builder flags its own defect post-merge** → hotfix atom, credited not
  punished; honesty is the currency.
- **Producer found real product rot on first read** → that's the system
  working; triage before celebrating.
- **Measurement artifact** (a "17 dead paths" that's really 1 + format noise)
  → fix the measurement, record the correction.
- **Drained queue** → the decomposition phase re-runs on the next journey
  lane; drained ≠ done.
