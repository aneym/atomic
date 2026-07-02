# atomic — its own gap queue (drive pass 1, 2026-07-02)

The skill run on itself. Ranked; each gap is claimable per CONTRIBUTING.md.

## Open gaps

1. **CORE-U2 — description trigger coverage untested.** The description's
   trigger phrases are authored, not observed. Gap: no record of which real
   phrasings auto-invoke vs miss. Atom: collect misses in practice; tune
   `description`/`when_to_use` from observed invocations, not intuition.
2. **REF-U4 — briefs assume a merge-train exists.** drive.md §5 presumes an
   auditor + batched verification; solo-maintainer repos have neither. Atom:
   a "minimum viable loop" paragraph — how drive works when builder and
   auditor are the same person separated only by time.
3. **TPL-U1 — no template validation script.** Templates are prose-validated
   only. Atom: `scripts/validate-templates.mjs` (zero-dep) checking JSON
   parses, TSV columns match decompose.md's table, and SKILL.md stays <500
   lines; wire as the repo's only CI-ish check.
4. **REF-U2 — producer honesty states lack a worked negative example.** The
   rubric doc names the fabricated-green sin but shows no concrete instance.
   Atom: add a 5-line before/after showing a producer that seeds its own rows
   vs one that reads real substrate.
5. **CORE-U1 — mode selection heuristic is repo-layout-coupled.** "No
   improvement-map exists" assumes one layout. Atom: name the generic marker
   (any committed rubric + units map, wherever they live) and tell the agent
   to ask/search before assuming bootstrap.
6. **DIST-U1 — no CHANGELOG.md yet.** CONTRIBUTING.md references it; it does
   not exist until the first behavior change. Atom: create on first version
   bump (intentionally deferred — an empty changelog is noise).

## Closed

- (none yet — this is pass 1)
