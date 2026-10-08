---
name: pr
description:
    Create a pull request for the current branch only when the user explicitly asks. Uses a conventional title and fills
    the repository's pull request template.
---

# Pull Request

## Steps

1. Run `git log main..HEAD --oneline` and `git diff main...HEAD` to understand all changes
2. Check if the branch tracks a remote: `git rev-parse --abbrev-ref --symbolic-full-name @{u} 2>/dev/null`
3. Push if needed: `git push -u origin <branch>`
4. Draft the PR:
   - **Title**: `type(scope): description` — conventional commit format, ≤ 70 chars
   - **Body**: follow `.github/PULL_REQUEST_TEMPLATE.md` section by section — tick the matching type, fill
     **Problem** and **Solution** with what changed and why, list real related links or drop the placeholder
     `Example link`, and tick the checklist only when true
   - Add the commands from the AGENTS.md verification table rows that match the diff, with their results
5. Create: `gh pr create --title "..." --body "$(cat <<'EOF' ... EOF)"`

## Never

- Force-push (`--force`)
- Use a title that doesn't follow conventional commit format
- Create a PR from `main` or `master`
