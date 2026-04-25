Create a conventional commit for the current changes.

1. Run `git status` and `git diff HEAD` to see all changes
2. Run `git log --oneline -5` to match recent commit style
3. Draft a commit message:
   - Format: `type(scope): subject` (imperative, lowercase, ≤ 70 chars)
   - Body: bullet points explaining WHY, wrapped at 80 chars
   - Add `BREAKING CHANGE:` footer only if applicable
4. Stage relevant files with specific paths (not `git add .`)
5. Commit using a heredoc to preserve formatting

Types: `feat` `fix` `docs` `style` `refactor` `perf` `test` `build` `ci` `chore` `revert`

Never use `--no-verify`. Never commit `.env` files.
