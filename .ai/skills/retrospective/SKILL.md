---
name: retrospective
description:
  Capture project lessons after corrections, surprises, or substantial sessions. Updates AGENTS.md (Lessons or
  Conventions) or README.md.
---

# Session Retrospective

## When to use

- User corrects your work (wrong path, wrong assumption, wrong approach)
- You hit a non-obvious tool limitation or project constraint
- User says "remember this", "add to lessons", or "document that"
- End of a substantial session with reusable project knowledge

## Steps

1. Identify what would have prevented the issue
2. Read `AGENTS.md` and `README.md`
3. Update the owner in place:
   - Gotchas and pitfalls go under `## Lessons` in `AGENTS.md` (add the section before `## AI workflow layout` if it
     does not exist)
   - Conventions go under `## Conventions` in `AGENTS.md`
   - Operational facts about using or testing the role go in `README.md`
4. Keep it as current fact, not a narrated history of the session; update or remove stale entries instead of
   duplicating them
5. Do not create a new documentation file

## What not to capture

- Trivial typo fixes
- One-off task details
- Information already obvious from the current code or the error message
