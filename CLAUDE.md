# Ansible Role — zsh

Ansible role that installs zsh and configures Oh My Zsh per-user.

@AGENTS.md

## For Claude Code

### Rules loaded automatically

| Rule file | Applied to |
|-----------|---|
| `.ai/rules/ansible.md` | `**/*.yml`, `**/*.yaml` |

### Constraints

- Oh My Zsh installs per-user — tasks must run as the target user (not root)
- Plugin and theme selection comes from variables in `defaults/main.yml` — never hardcode them in tasks
- Test idempotency: running the role twice must produce no changes on the second run
