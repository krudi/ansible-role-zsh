# ansible-role-zsh

A role for [Ansible](https://github.com/ansible/ansible), that installs [zsh](https://www.zsh.org/) and [Oh My Zsh](https://ohmyz.sh/) for better Terminal workflow.

## Requirements

This role does not need any additional required packages.

## Quick start

1. First clone this repository and add into your project directory.
2. Include the role in your [Ansible](https://github.com/ansible/ansible) playbook.

## Example playbook

Example use of a role, that will install the latest version of [zsh](https://www.zsh.org/) and [Oh My Zsh](https://ohmyz.sh/).

```yml
- hosts: all
  roles:
    - role: krudi.zsh
      zsh:
        write_file: true
        change_shell: true
      omz:
        install: true
        user: bob
        plugins:
          - git
          - zsh-autosuggestions
```

## Role variables

| Variable            | Default                          | Description                                              |
| -------------------- | ----------------------------------- | ------------------------------------------------------------ |
| `zsh.write_file`     | `true`                             | Whether to write the user's `~/.zshrc` from the role template. |
| `zsh.change_shell`   | `true`                             | Whether to change the user's default shell to zsh.          |
| `omz.install`        | `true`                             | Whether Oh My Zsh should be installed.                      |
| `omz.user`           | `bob`                              | The user Oh My Zsh (and the `.zshrc`) is installed for.     |
| `omz.plugins`        | `["git", "zsh-autosuggestions"]`   | The Oh My Zsh plugins enabled in the generated `.zshrc`.    |

See `defaults/main.yml` for the current defaults. Note `omz.user` defaults to `bob`, which you will typically want to override for your own user.

## Testing

This role includes a [Molecule](https://ansible.readthedocs.io/projects/molecule/) test scenario under `molecule/default`. Run it with `molecule test` (requires Docker and provisions real containers, so run it deliberately rather than as part of routine checks).

## Issue

Have you found a bug in this project or have a suggestion for a new feature? Create a new ticket for the bug or feature, which can be found on the [GitHub](https://github.com/krudi/ansible-role-zsh/issues) page.
