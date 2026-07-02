# Phase A — Decompose

## Contents
1. Lanes (journey order)
2. Units (atomic, seam-bound)
3. Join tables as data
4. Common decomposition mistakes

## 1. Lanes (journey order)

A **lane** is a slice of the product a user experiences in sequence. Order lanes
by the user journey, not by architecture: if the first lane is bad, everything
downstream is bad regardless of its own quality (e.g. for a job-search product:
jobs pipeline → matching → surfacing → apply → conversation). 4–7 lanes is
typical; more means your lanes are units.

Each lane gets: a readiness rubric (phase B), a gap queue, a monitor (a
deterministic health check that runs on a schedule and files issues), and a
history page in the project's knowledge base.

## 2. Units (atomic, seam-bound)

A **unit** is the smallest independently improvable thing: one behavior, one
seam, one eval. The test: could one agent improve this unit in isolation, prove
it with a focused eval, and merge without coordinating beyond its seam?

`units.tsv` columns (tab-separated, checked in; see `templates/units.example.tsv`):

| column | meaning |
|---|---|
| `track` | grouping code (e.g. SUPPLY, MATCH) — maps to a lane via the join table |
| `unit_id` | stable id, unique within track (key rows by `(track, unit_id)` — ids WILL collide across tracks) |
| `title` | one line, plain language |
| `seam_paths` | comma-free, repo-relative paths this unit owns (files or dirs) |
| `metric_bar` | committed numeric bar IF a real metric exists — else honest blank |
| `status` | active \| planned \| superseded (never delete rows — the map is a record) |

Seam rules: every unit lists the exact paths it may touch. Two units sharing a
path cannot run in parallel waves. Validate seam paths exist on disk as a
readiness criterion (dead paths = map rot).

## 3. Join tables as data

All joins are checked-in TSV/JSON files, never constants in code:
- `track-lane-map.tsv` — track → lane
- `feature-lane-map.tsv` — feature/tracker id → lane (if a feature tracker exists)

Data files merge cleanly across parallel lanes, diff reviewably, and let any
tool (or agent) join without importing project code.

## 4. Common decomposition mistakes

- **Architecture-shaped lanes** ("backend", "frontend") — useless for journey
  reasoning. Decompose by what the user experiences.
- **Units without seams** — if you can't name the files, it isn't a unit yet;
  split or research first.
- **Deleting superseded rows** — flag `status: superseded`; the map doubles as
  history.
- **Inventing metric bars** — a blank bar is honest; a made-up bar poisons the
  rubric. Fill bars only from metrics that already exist.
