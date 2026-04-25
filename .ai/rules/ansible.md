---
description: Ansible role conventions for this project
globs: ["**/*.yml", "**/*.yaml"]
alwaysApply: false
---

# Ansible Role Conventions

## Directory structure

```
tasks/main.yml         # entry point, includes other task files
handlers/main.yml
defaults/main.yml      # default variable values (lowest precedence)
vars/main.yml          # internal variables
meta/main.yml          # role metadata and dependencies
templates/*.j2         # Jinja2 templates
files/                 # static files
```

## Naming conventions

- Tasks: descriptive, sentence case (`Install nvm dependencies`)
- Variables: `snake_case`, prefixed with role name (`nvm_version`, `nvm_install_dir`)
- Handlers: descriptive action (`Restart nginx`)

## Best practices

- Use fully qualified collection names (FQCN): `ansible.builtin.package` not `package`
- Always define defaults for every variable you use in `defaults/main.yml`
- Use `become: true` only where needed — not at play level if only some tasks need it
- Prefer `ansible.builtin.template` over `copy` for config files that vary per host

## Commands

```bash
ansible-lint      # lint the role
molecule test     # full test cycle
molecule converge # apply role without destroy
```
