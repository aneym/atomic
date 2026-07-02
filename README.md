# atomic

**A framework for atomic improvement loops in complex repos.**

Big products rot when improvement is monolithic: everything depends on
everything, evals can't be trusted, and "what should we work on next?" is a
meeting instead of a query. `atomic` turns a complex repo into a system that
improves itself:

- **Lanes** ordered by the user journey (upstream quality gates everything downstream)
- **Atomic units** with explicit file-path seams, so agents improve pieces in
  isolation and merge without collisions
- **Readiness rubrics** per lane — deterministic, data-measurable criteria scored
  by producers that are structurally unable to lie (`producer: unbuilt` is an
  honest red, never a fabricated green)
- **A hard-eval harness** — RED-first atoms, isolated sandboxes, an append-only
  runs ledger, flow-level stories, and independent re-verification
- **A drive loop** — score → rank gaps → emit dispatch-ready atom briefs any
  agent can execute → re-score and show the Δ

It is an [Agent Skill](https://code.claude.com/docs/en/skills): a procedure the
agent follows, plus reference files and templates it reads on demand. It ships
no runtime — your project keeps its own tiny scorer; the skill supplies the
method, schemas, and integrity rules.

## Install

```bash
git clone https://github.com/aneym/atomic ~/.claude/skills/atomic
```

Then in any project: invoke `/atomic` (or just describe the goal — "break this
into atomic improvement lanes with hard evals" auto-invokes it).

## The two modes

| Mode | When | What happens |
|---|---|---|
| **Bootstrap** | first run on a project | decompose → rubrics + producers → harness conventions |
| **Drive** | every run after | score lanes → regression tripwire → ranked gap queue → atom briefs → Δ |

## The integrity rules (why the grades can be trusted)

1. Builders never self-certify — a different executor re-runs every proof.
2. Producers read real substrate only; honesty states are explicit
   (green / red / unbuilt / infra-skip).
3. Measurement corrects itself via versioned category corrections — never by
   thinning thresholds.
4. Judgment labels come from the highest-judgment agent; machinery is delegable.
5. One owner per seam; parallel waves are seam-disjoint by construction.

## Layout

```
SKILL.md                 the procedure (modes, checklists, non-negotiables)
reference/decompose.md   lanes, units, seams, join tables
reference/rubrics.md     criteria authoring, producer honesty, maturity ladder
reference/harness.md     RED-first atoms, runs ledger, sandboxes, re-verification
reference/drive.md       ranking, atom briefs, merge train, loop feedback
templates/               rubric / units / atom-brief starting points
```

Reference implementation: [aneym/iris](https://github.com/aneym/iris) — the
first product run entirely on this loop (readiness map, runs ledgers, neutral
re-verifier, and a fleet of parallel lane agents).

MIT.
