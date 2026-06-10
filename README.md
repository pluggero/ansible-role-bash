# ansible-role-bash

[![CI](https://github.com/pluggero/ansible-role-bash/actions/workflows/ci.yml/badge.svg)](https://github.com/pluggero/ansible-role-bash/actions/workflows/ci.yml) [![Ansible Galaxy downloads](https://img.shields.io/ansible/role/d/pluggero/bash?label=Galaxy%20downloads&logo=ansible&color=%23096598)](https://galaxy.ansible.com/ui/standalone/roles/pluggero/bash)

Installs bash and manages shell configuration files on ArchLinux and Debian.

## Requirements

**Collections:**

- `ansible.builtin` (included with Ansible)
- `community.general` (for ArchLinux support)

**Role dependencies:**

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
bash_install_method: "package"
```

Installation method. Only `"package"` is supported.

```yaml
bash_config_dir: "{{ ansible_env.HOME }}"
```

Directory where bash config files are deployed (`~/.bashrc`, `~/.bash_profile`, etc.).

```yaml
bash_enable_bashrc: true
bash_enable_bash_profile: true
bash_enable_aliases: true
bash_enable_functions: true
```

Feature flags controlling which config files are managed. Set any to `false` to skip managing that file.

```yaml
bash_histsize: 10000
bash_histfilesize: 20000
```

Controls `HISTSIZE` and `HISTFILESIZE` in `~/.bashrc`.

```yaml
bash_aliases: []
# - name: ll
#   command: "ls -lah"
```

List of aliases to define in `~/.bash_aliases`. Each entry must have `name` and `command` keys.

```yaml
bash_functions: []
# - name: mkcd
#   body: |
#     mkdir -p "$1" && cd "$1"
```

List of shell functions to define in `~/.bash_functions`. Each entry must have `name` and `body` (multiline string).

```yaml
bash_env_vars: {}
# EDITOR: nvim
# PAGER: less
```

Dict of environment variables to export in `~/.bashrc`.

```yaml
bash_path_additions: []
# - "{{ ansible_env.HOME }}/.local/bin"
```

List of paths to prepend to `PATH` in `~/.bashrc`. Uses deduplication logic to avoid double-adding paths.

```yaml
bash_login_shell_commands: []
# - "exec sway"
```

List of commands to include in `~/.bash_profile` (login-shell only).

```yaml
bash_extra_config: []
# - "eval \"$(zoxide init bash)\""
```

List of raw lines appended verbatim to `~/.bashrc`.

```yaml
bash_source_files: []
# - "{{ ansible_env.HOME }}/.env_vars"
```

List of files to source in `~/.bashrc` (each guarded by `[ -f ... ]`).

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: pluggero.bash
      vars:
        bash_env_vars:
          EDITOR: nvim
          PAGER: less
        bash_aliases:
          - name: ll
            command: "ls -lah"
          - name: gs
            command: "git status"
        bash_functions:
          - name: mkcd
            body: |
              mkdir -p "$1" && cd "$1"
        bash_path_additions:
          - "{{ ansible_env.HOME }}/.local/bin"
        bash_extra_config:
          - "eval \"$(zoxide init bash)\""
```

## License

MIT

## Author Information

pluggero, 2025-present
