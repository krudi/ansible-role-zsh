---
name: retrospective
description: Capture lessons learned after a session. Use when the user corrects your work, after surprises, or at the end of a substantial session. Updates .ai/memory/lessons.md.
---

# Session Retrospective

## When to use

- User corrects your work (wrong path, wrong assumption, wrong approach)
- You hit a non-obvious tool limitation or project constraint
- User says "remember this", "add to lessons", or "document that"
- End of a session with substantial corrections

## Steps

1. Identify what to capture — for each correction, ask: what would have prevented this?
2. Read `.ai/memory/lessons.md` to check existing lessons
3. Write or update lessons:
   - Group by category: Patterns, Gotchas, Commands, Architecture, etc.
   - Format: `**Bold trigger**: actionable one-sentence lesson`
   - Max 1–2 sentences per lesson
   - Update existing entries instead of duplicating
   - Remove stale/wrong lessons
4. Keep the file under ~80 lines total — consolidate if it grows

## What NOT to capture

- Trivial typo fixes
- One-off task details
- Things already obvious from the error message
