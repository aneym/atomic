# Phase B — Rubrics and producers

## Contents
1. Rubric shape
2. Criteria authoring rules
3. Producer types and honesty states
4. Maturity ladder and north star
5. Changing a rubric legitimately

## 1. Rubric shape

One rubric file per project (`readiness.json`), one block per lane, plus a
SYSTEM block for the harness itself (the loop must grade its own machinery too).
See `templates/rubric.example.json`. Every criterion:

```json
{
  "id": "SUPPLY-U2",
  "title": "Recent-active pool freshness",
  "rationale": "Users must never see week-old dead inventory.",
  "producer": { "type": "command", "run": "node scripts/ops/freshness.mjs", "metric": "fresh_share" },
  "threshold": { "op": "gte", "value": 0.9 },
  "level": 2,
  "weight": 1
}
```

`producer.type` is generic: `command` (any shell command emitting JSON with the
metric), `runs-ledger` (read the newest matching row from the runs ledger), or
`unbuilt` (see below). The skill ships no runner — the project's scorer is a
small script that walks the rubric, executes producers, compares thresholds,
and writes a snapshot. Keep it zero-dependency.

## 2. Criteria authoring rules

- **Deterministic and DB/data-measurable first.** A criterion an agent can
  compute from the project's own data beats one needing a model or a human.
- **One observable per criterion.** "Fresh AND deduped AND ranked" is three.
- **Anchor to user harm.** The rationale names what a user experiences when red.
- **Thresholds from current reality + a ratchet**, never aspiration. A rubric
  that starts all-red from invented bars teaches everyone to ignore it.
- **Non-vacuous by proof**: for every new criterion, show it can fail — inject a
  violation and watch it go red before you trust its green.
- **Category discipline**: a lane rubric measures the PRODUCT lane; work-process
  hygiene (lane files, briefs, CI) belongs in the SYSTEM block, not product lanes.

## 3. Producer types and honesty states

| state | meaning | rule |
|---|---|---|
| bound + green | producer read real substrate, threshold met | trust only after the non-vacuity proof |
| bound + red | real substrate, threshold unmet | this is the backlog — never massage it |
| `unbuilt` | the substrate or producer doesn't exist yet | scores as fail; the gap queue turns it into a build atom |
| infra-skip | producer couldn't reach its substrate (env/deployment absent) | visible skip with reason; never a fail, never a pass |

The cardinal sin: a producer that fabricates substrate (seeds fake rows, spins a
private harness, invents labels) to green a criterion. An honest `unbuilt` is a
dispatchable atom; a fabricated green is invisible rot.

## 4. Maturity ladder and north star

Each lane climbs L0→L4 (exists → measured → gated → monitored → self-healing).
A level is held only when every criterion at that level passes — percentage
scores may rise without the ladder moving, and that is correct (no gaming:
report both). Define one **north star** per project: the end-to-end outcome
(e.g. "a user's chain from signup to submitted application closes headlessly")
counted only from lanes at L2+.

## 5. Changing a rubric legitimately

Version every rubric (`r0.1`, `r0.2`…). Two legal change classes:
1. **Category correction** — the criterion measured the wrong kind of thing;
   rewrite it, bump the version, note the correction.
2. **Ratchet** — raising a threshold after sustained green.

Illegal: lowering a threshold or narrowing a criterion so a red turns green
without the product changing. If a criterion is UNSATISFIABLE as written (the
data model can't express it), redefining it is a category correction — say so
explicitly and record what was dropped.
