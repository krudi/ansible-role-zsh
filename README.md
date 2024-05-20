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
        write_file: false
      omz:
        install: true
        user: bob
        plugins:
          - git
          - zsh-autosuggestions
```

## Issue

Have you found a bug in this project or have a suggestion for a new feature? Create a new ticket for the bug or feature, which can be found on the [GitHub](https://github.com/krudi/ansible-role-zsh/issues) page.
