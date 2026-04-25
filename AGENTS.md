# Ansible Role — zsh

Ansible role that installs zsh and configures Oh My Zsh for an improved terminal workflow.

## Stack

- Ansible / YAML

## Commands

```bash
ansible-lint       # lint the role
molecule test      # full test (if molecule configured)
```

## Onboarding

**Prerequisites:** Ansible, molecule.

1. `ansible-lint` — verify linting passes
2. `molecule converge` — apply the role
3. `molecule test` — full test including idempotency

---

## Testing

- Run before every PR: `ansible-lint && molecule test`
- Verify Oh My Zsh is installed and the configured shell is set as default for the target user

---

## Notes

- Oh My Zsh installs per-user — tasks run as the target user
- Plugin and theme selection controlled by variables (see `defaults/main.yml`)

---

## Rules

@.ai/rules/ansible.md

---

## For Claude Code

### Rules loaded automatically

| Rule file | Applied to |
|-----------|---|
| `.ai/rules/ansible.md` | `**/*.yml`, `**/*.yaml` |

### Constraints

- Oh My Zsh installs per-user — tasks must run as the target user (not root)
- Plugin and theme selection comes from variables in `defaults/main.yml` — never hardcode them in tasks
- Test idempotency: running the role twice must produce no changes on the second run
