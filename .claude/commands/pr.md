Create a pull request for the current branch.

1. Run `git status`, `git log main..HEAD --oneline`, and `git diff main...HEAD` to understand all changes
2. Check if branch is pushed: `git rev-parse --abbrev-ref --symbolic-full-name @{u}` (push with `-u` if not)
3. Draft the PR:
   - Title: conventional commit format `type(scope): description` (≤ 70 chars)
   - Summary: 2–3 bullet points on what changed and WHY
   - Test plan: checklist of what to verify before merging
4. Create with: `gh pr create --title "..." --body "..."`

Never force-push. Title must follow conventional commit format.
