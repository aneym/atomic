# Atom brief — <lane>/<short-name>

**Criterion:** <RUBRIC-ID> — <title> (current: <red/unbuilt>, threshold: <op value>)
**Why now:** <journey/maturity/regression rationale in one line>

## Behavior change
<One concrete behavior, stated as observable outcome — not implementation.>

## Seam (owned paths — touch nothing else)
- <path/one>
- <path/two>
Forbidden: <paths other live lanes own>

## Eval (RED first)
1. Write the focused test asserting the new behavior; commit it FAILING
   (`test(<lane>): RED — <behavior>`).
2. Implement until green: `<exact eval command>`
3. If stateful: prove in an isolated sandbox, never the shared deployment.

## Done means
- Focused eval green (parsed counts, not exit-code folklore)
- Runs-ledger row written (suite, pass/fail/infra_skip, metrics, unit_id)
- No thinned predicates; honest infra-skips visible
- PR opened, NOT merged (the auditor re-runs and merges)

## Report envelope (message back)
PR #, files touched, parsed test counts, what remains unproven and why,
residual risks enumerating the input classes actually checked.
