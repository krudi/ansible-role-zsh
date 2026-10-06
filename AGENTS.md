# Agent instructions

This file is the canonical instruction source for every coding agent. `CLAUDE.md` only imports it, because Claude Code
does not read `AGENTS.md` by itself. Start every agent session at the repository root: Claude Code expands the
`@AGENTS.md` import only for the working directory's `CLAUDE.md`, and Codex started inside a subdirectory loads only
that directory's `AGENTS.md` and none of the repository skills. Skills, adapters and nested instruction files point
here; they never restate rules.

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
| Repeatable workflows (commit, PR, retrospective) | `.ai/skills/<name>/SKILL.md` — pick by its description |

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
- `templates/zshrc.j2` renders `~/.zshrc`; its theme is fixed to `robbyrussell` and its `plugins=()` line joins
  `omz.plugins`.
- Use fully qualified collection names (`ansible.builtin.package`, not `package`).
- Task names are descriptive sentence case; handler names describe the action.
- Variables are `snake_case`. Public variables live in a dict named after the tool (`zsh.write_file`, `omz.user`);
  internal variables in `vars/` are prefixed (`omz_dependencies`).
- Every public variable has a default in `defaults/main.yml`, an entry in `meta/argument_specs.yml` and a row in the
  `README.md` variables table; change all three together.
- Set `become: true` on the tasks that need it, never at play level.
- Prefer `ansible.builtin.template` over `ansible.builtin.copy` for config files that vary per host.
- Adding a platform means updating `meta/main.yml`, the platforms in `molecule/default/molecule.yml`, the matrix in
  `.github/workflows/tests.yaml` and `README.md` together.
- `ansible-lint` runs the default rules with `yaml` and `role-name` skipped (`.ansible-lint`). YAML is indented with 2
  spaces, workflow files with 4 (`.editorconfig`).

## Working rules

- Run commands from the repository root. The Python tooling is pinned in `requirements-ci.txt` (Ansible, Molecule and
  its Docker driver) and `requirements-release.txt`; CI uses Python 3.11 and runs `ansible-lint` through the
  `ansible/ansible-lint` action. Do not install or upgrade tooling unless asked.
- Never commit, amend or push unless asked. Stage only files that belong to the task; leave unrelated dirty files,
  owner-local files and untracked scratch exactly as found — never restore, reset or stash them to make a task easier.
- Generated output (`*.retry`, `.cache/`, `.venv/`, Molecule and ansible-lint state) is never committed.
- Molecule creates and destroys its own Docker containers (`instance-ubuntu2404`, `instance-ubuntu2204`,
  `instance-debian12`, `instance-debian13`). Never start, stop or remove any other container on the host; after an
  interrupted run, clean up only with `molecule destroy`.
- Do not modify unrelated files solely to make a check pass, and never silently skip a failing check: report the exact
  command, the failure, and whether it looks related. If `ansible-lint` or `molecule` is not installed, say so and give
  the command for the owner instead.

## Verification

Verification is proportional to the change. While iterating, run the row(s) that match the files you touched; before
declaring a change complete, widen to every area it crosses. `molecule test` provisions real containers, so run it
deliberately rather than as a routine check.

| Changed area                                                       | While iterating                                        | Before completion, when applicable                                      |
| ------------------------------------------------------------------ | ------------------------------------------------------ | ----------------------------------------------------------------------- |
| `tasks/`, `handlers/`, `templates/`, `defaults/`, `vars/`, `meta/` | `ansible-lint`                                         | `molecule test`                                                         |
| `molecule/`                                                        | `ansible-lint`; `molecule converge && molecule verify` | `molecule test`                                                         |
| `.github/workflows/`                                               | —                                                      | The `Lint` and `Test (Molecule)` workflows on the PR                    |
| `README.md`, `AGENTS.md`, `.ai/`                                   | —                                                      | Variable tables match `defaults/main.yml` and `meta/argument_specs.yml` |

`molecule test` runs the full `test_sequence` in `molecule/default/molecule.yml` (syntax, converge, idempotence,
verify, destroy) on all four platforms; without `MOLECULE_DISTRO` each instance uses its own distro image. CI sets
`MOLECULE_DISTRO` per matrix entry and also runs weekly.

## AI workflow layout

| Path                                          | Holds                                                                                                                          |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `.ai/skills/<name>/`                          | Project skills — the only copy of each; generated files go to the skill's own `output/`, ignored by a skill-local `.gitignore` |
| `.agents/skills/`                             | Third-party skills managed by `npx skills` (`skills-lock.json`), plus a symlink per project skill so Codex discovers it        |
| `.claude/skills/`                             | Symlinks only — Claude Code's discovery path into both of the above                                                            |
| `.claude/settings.json`, `.codex/config.toml` | Per-agent settings only; keep their environment policy in sync                                                                 |
| `.ai/audits/<YYYY-MM-DD>-<slug>/README.md`    | An audit lives here only while it holds unresolved work                                                                        |

## Project skills

All ansible-role-zsh-owned skills live canonically under `.ai/skills/`.

Agent-specific skill directories such as `.claude/skills/` and `.agents/skills/` must contain only adapters or symlinks
to those project skills when required for tool discovery (`ln -s ../../.ai/skills/<name>` in both).

Never maintain duplicate copies of an ansible-role-zsh-owned `SKILL.md`.

When an audit is done, move its durable conclusions into `AGENTS.md` or `README.md`, carry any open item to a tracked
place, and delete the folder — git history is the archive. Audits carry no screenshots or raw dumps.
