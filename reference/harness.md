# Phase C — The hard-eval harness

## Contents
1. Eval-first atoms (RED → GREEN)
2. The runs ledger
3. Isolated sandboxes
4. Flow-level evals (stories)
5. Independent re-verification
6. Tracker write-back

## 1. Eval-first atoms (RED → GREEN)

Every improvement atom starts with a failing focused test committed BEFORE the
fix (`test(<lane>): RED — <behavior>`), then the fix flips it green. The RED
commit is the proof the eval can fail — a test never seen red proves nothing.
One seam, one behavior change, one focused test per atom.

## 2. The runs ledger

Append-only JSONL, one row per eval/verification run — the durable substrate
readiness producers read. Minimum row shape (see `templates/atom-brief.md` for
the reporting contract):

```json
{"at":"<iso>","producer":"<who ran it>","branch":"<branch>","suite":"<eval id>",
 "pass":N,"fail":N,"infra_skip":N,"metrics":{"<metric>":<value>},
 "unit_id":"<TRACK:ID when derivable>","sha":"<commit>"}
```

Rules: writers never overwrite; the newest matching row wins for scoring;
store the ledger somewhere durable (outside any directory that gets reset —
learned the hard way: an automated tree-reset wiped an uncommitted ledger).
A repo copy for visibility is fine; the durable home is canonical.

## 3. Isolated sandboxes

Stateful evals run against a per-lane isolated backend (own DB, own ports,
loopback-only), never the shared/live deployment. Non-negotiables learned in
production:
- Sandbox bootstrap must set EVERY env/secret the code path needs — a missing
  minor secret (a link-signing key) silently zeroed an entire eval step for
  days while production worked. When a sandbox eval fails where prod works,
  suspect sandbox env first.
- Worktrees/sandboxes must be STRUCTURALLY unable to deploy code to the shared
  environment (guard shims over the deploy CLI, filtered env copies). A bare
  codegen once full-pushed a worktree to shared.
- Long-lived shared fixtures (a mock server) accumulate state across runs; any
  eval asserting a virgin fixture must reset it at start.

## 4. Flow-level evals (stories)

Unit evals miss integration rot. Encode user flows as data-driven stories:
ordered beats (`say`/`act`/`read`/`seed`) with predicates over outcomes
(`matches`, `exists`, `includes`, widget/card kinds). Classification contract:
- Model/gateway unreachable, timeouts, aborts → **infra-skip** with reason.
- A genuine predicate failure → **fail**, loudly.
- Never fake green; never let an infra error masquerade as a product fail.
Check the INNER result of wrapped verbs — an `{ok:false}` inside a 200 envelope
classified as product-fail was a false-RED generator until fixed.

## 5. Independent re-verification

A separate scheduled verifier (different executor than the builder) replays the
story corpus and the end-to-end journey from an out-of-repo clean baseline,
compares against recorded rows, and writes agree/disagree counts. Builders
never self-certify: the auditor reruns focused suites before any merge; batch
expensive checks (typecheck, lint) once per merge wave, not per atom.

## 6. Tracker write-back

If a feature tracker exists, the story runner writes `test_status` back per
covered feature (idempotent — second run changes 0 cells; failing story wins
over passing). Coverage becomes a ratchet criterion: declared coverage may
never drop.
