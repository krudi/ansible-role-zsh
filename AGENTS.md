# Agent instructions

`ansible-role-zsh` is an Ansible role, published to Ansible Galaxy as `krudi.zsh`, that installs zsh and Oh My Zsh
and writes the user's `~/.zshrc` from `templates/zshrc.j2`.

## Where knowledge lives

| Question                                         | Source                                                 |
| ------------------------------------------------ | ------------------------------------------------------ |
| What the role does, how to use it, its variables | `README.md`                                            |
| Variable defaults and their types                | `defaults/main.yml`, `meta/argument_specs.yml`         |
| Supported platforms and role dependencies        | `meta/main.yml`                                        |
| Test scenario (platforms, sequence, assertions)  | `molecule/default/`                                    |
| CI lint, test matrix and Galaxy release          | `.github/workflows/`                                   |
| Branch and commit message conventions            | `.github/CONTRIBUTING.md`                              |
| Repeatable workflows (commit, PR)                | `.ai/skills/<name>/SKILL.md` — pick by its description |
| Third-party skills                               | `skills-lock.json` (`npx skills`)                      |

This file owns the agent conventions. Do not create competing documentation; update the owner instead.

## Non-negotiables

- The role is idempotent: a second run reports no changes. Molecule's `idempotence` step enforces it.
- Only Ubuntu jammy/noble and Debian bookworm/trixie are supported; every include in `tasks/main.yml` is guarded by
  `ansible_os_family == "Debian"`.
- The role has no role dependencies: `dependencies` in `meta/main.yml`, `requirements.yml` and
  `molecule/default/requirements.yml` stay empty unless one is added to all three.
- Oh My Zsh and `~/.zshrc` belong to `omz.user` (default `bob`, created by `molecule/default/prepare.yml`); the tasks
  reach that user with `become_user: "{{ omz.user }}"`. Plugins come only from `omz.plugins`; never hardcode them.
- Never read or print `.env` values or other secrets, including the `ANSIBLE_GALAXY_API_KEY` release secret.
- Do not add explanatory or rationale comments in code; explain in chat or in `README.md` instead.

## Conventions

- `tasks/main.yml` is the entry point and only includes the `setup-*.yml` task files. `defaults/main.yml` holds the
  public variables (lowest precedence), `vars/main.yml` the internal ones, `meta/main.yml` the Galaxy metadata and
  dependencies, `templates/*.j2` Jinja2 templates and `files/` static files.
- Task names are descriptive sentence case; handler names describe the action.
- Variables are `snake_case`. Public variables live in a dict named after the tool (`zsh.write_file`, `omz.user`);
  internal variables in `vars/` are prefixed (`omz_dependencies`).
- Every public variable has a default in `defaults/main.yml`, an entry in `meta/argument_specs.yml` and a row in the
  `README.md` variables table; change all three together.
- Set `become: true` on the tasks that need it, never at play level.
- Prefer `ansible.builtin.template` over `ansible.builtin.copy` for config files that vary per host.
- Adding a platform means updating `meta/main.yml`, the platforms in `molecule/default/molecule.yml`, the matrix in
  `.github/workflows/tests.yaml` and `README.md` together.

## Working rules

- Run commands from the repository root. Run the Python tooling (pinned in `requirements-ci.txt`) through `uvx` as
  shown in Verification; do not install or upgrade tooling globally unless asked.
- `uvx lefthook install` enables the `lefthook.yml` pre-commit hook; never bypass it with `--no-verify`.
- Never commit, amend or push unless asked. Stage only files that belong to the task; leave unrelated dirty files,
  owner-local files and untracked scratch exactly as found — never restore, reset or stash them to make a task easier.
- Molecule creates and destroys its own `instance-*` Docker containers. Never start, stop or remove any other container
  on the host; after an interrupted run, clean up only with `molecule destroy`.
- Do not modify unrelated files solely to make a check pass, and never silently skip a failing check: report the exact
  command, the failure, and whether it looks related. If Docker is unavailable for Molecule, say so and give the command
  for the owner instead.

## Verification

Verification is proportional to the change. While iterating, run the row(s) that match the files you touched; before
declaring a change complete, widen to every area it crosses. `molecule test` provisions real containers, so run it
deliberately rather than as a routine check. Both tools run at the CI-pinned versions without a global install:
`ansible-lint` is `uvx --with-requirements requirements-ci.txt ansible-lint`, and `molecule <command>` is
`uvx --with-requirements requirements-ci.txt --from molecule molecule <command>`.

| Changed area                                                       | While iterating                                        | Before completion, when applicable                                      |
| ------------------------------------------------------------------ | ------------------------------------------------------ | ----------------------------------------------------------------------- |
| `tasks/`, `handlers/`, `templates/`, `defaults/`, `vars/`, `meta/` | `ansible-lint`                                         | `molecule test`                                                         |
| `molecule/`                                                        | `ansible-lint`; `molecule converge && molecule verify` | `molecule test`                                                         |
| `.github/workflows/`                                               | —                                                      | The `Lint` and `Test (Molecule)` workflows on the PR                    |
| `README.md`, `AGENTS.md`, `.ai/`                                   | —                                                      | Variable tables match `defaults/main.yml` and `meta/argument_specs.yml` |

`molecule test` runs the `test_sequence` in `molecule/default/molecule.yml` on every platform;
`--platform-name instance-<distro>` limits it to one, as each CI matrix job does.
