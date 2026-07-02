# Contributing to atomic

Improvements are welcome — from humans and from agents. This file is the
contract; PRs that follow it get reviewed fast, and most that carry real
evidence get merged.

## The PR contract (for you or your agent)

Every PR is **one atom**: one improvement, one concern, evidence included.

1. **State the gap as a criterion.** What does the skill get wrong or leave
   ambiguous today? Phrase it falsifiably ("an agent following drive.md cannot
   tell X" beats "drive.md could be clearer").
2. **Show the failure.** The strongest evidence is a transcript excerpt or a
   concrete scenario where an agent following the current skill did the wrong
   thing. Second best: a worked example the current text cannot produce.
3. **Make the smallest change that fixes it.** One file where possible.
   Respect the structure: SKILL.md stays under 500 lines; depth goes in
   `reference/`; reference files stay one level deep and get a table of
   contents past 100 lines.
4. **Show the after.** How does the same scenario resolve with your change?
5. **Don't fork the method.** Changes that weaken the integrity rules
   (self-certification, fabricated producers, threshold-thinning, label
   delegation) are rejected regardless of polish. Propose a new rule as an
   addition with its own rationale instead.

PR description template:

```
Gap: <falsifiable statement of what fails today>
Evidence: <scenario/transcript showing it>
Change: <files touched, one line each>
After: <the same scenario under the new text>
```

## Review outcomes

- **Merge** — gap is real, evidence holds, change is minimal and in-structure.
- **Reject with reason** — most common: no evidence, multiple concerns in one
  PR, integrity-rule weakening, or depth added to SKILL.md instead of
  `reference/`.

## Maintainer flow (repo owner)

Owner-directed agents fold improvements in directly: commit to `main`, bump
`version` in SKILL.md frontmatter on behavior changes, and note the change in
`CHANGELOG.md`. Installs are plain git clones — update any machine with:

```bash
git -C ~/.claude/skills/atomic pull
```

## The skill improves itself

This repo runs its own method on itself: `.atomic/` holds the skill's own
units map, criteria, and current gap queue. Before proposing a change, check
whether `.atomic/gaps.md` already names it — claiming an open gap with
evidence is the fastest path to a merge.
