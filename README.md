# Varnish Ansible Role

[![CI](https://github.com/bilalcaliskan/varnish-ansible-role/workflows/CI/badge.svg?event=push)](https://github.com/bilalcaliskan/varnish-ansible-role/actions?query=workflow%3ACI)
[![GitHub tag](https://img.shields.io/github/tag/bilalcaliskan/varnish-ansible-role.svg)](https://GitHub.com/bilalcaliskan/varnish-ansible-role/tags/)

Installs and configures Varnish cache server on RHEL/CentOS 7/8 instances.

## Requirements
This role has below requirements:
- [Python 3.x](https://www.python.org/downloads/)
- [Ansible](https://docs.ansible.com/) (min 2.4, suggested 2.9.16)

You can install suggested version with pip3:
```
$ pip3 install "ansible==2.9.16"
```

Note that this role requires root access, so either run it in a playbook with a global `become: true`, or invoke the role in your playbook.

## Role Variables
See the default values in [defaults/main.yml](defaults/main.yml). You can overwrite them in [vars/main.yml](vars/main.yml) if neccessary or you can set them while running playbook.

> Please note that this role can ensure that `firewalld` systemd service on your servers are started and enabled by default. If you want to start and enable `firewalld` service, please modify below variable as true while running playbook:
> ```yaml
> firewalld_enabled: true
> ```

## Dependencies

None

## Examples

### Inventory
```
[varnish]
node01.example.com
node02.example.com
node03.example.com
```

### Installation
```yaml
- hosts: varnish
  become: true
  roles:
    - role: bilalcaliskan.varnish
      vars:
        varnish_install: true
        purging_enabled: true
        banning_enabled: true
        version: 60lts
        port: 6081
        limit_memlock: 85983232
        limit_core: infinity
        limit_nofile: 131072
        storage_backend: malloc
        storage_backend_size: 256m
        global_ttl: 5m
        global_grace: 30m
        backends:
          - name: default
            host: 127.0.0.1
            port: 8080
            max_connections: 800
            first_byte_timeout: 600s
            connect_timeout: 600s
            between_bytes_timeout: 600s
            probe_enabled: true
            probe:
              url: "/"
              timeout: 5s
              interval: 30s
              window: 5
              threshold: 3
            requests:
              - url: "/"
                ttl: 1m
                grace: 10m
                keep: 20m
          - name: default2
            host: 127.0.0.1
            port: 8081
            max_connections: 800
            first_byte_timeout: 600s
            connect_timeout: 600s
            between_bytes_timeout: 600s
            probe_enabled: true
            probe:
              url: "/"
              timeout: 5s
              interval: 30s
              window: 5
              threshold: 3
            requests:
              - url: "/"
```

### Uninstallation

```yaml
- hosts: varnish
  become: true
  roles:
    - role: bilalcaliskan.varnish
      vars:
        varnish_install: false
```

## Development
This project requires below tools for development:
- [Python 3.x](https://www.python.org/downloads/)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) - (min 2.4, suggested 2.9.16)
- [pre-commit](https://pre-commit.com/)
- [ansible-lint](https://ansible-lint.readthedocs.io/en/latest/installing.html#using-pip-or-pipx) - required by [pre-commit](https://pre-commit.com/)
- [Bash shell](https://www.gnu.org/software/bash/) - required by [pre-commit](https://pre-commit.com/)

After you install all the tools above, you can simply configure [pre-commit](https://pre-commit.com/) by typing:
```shell
$ pre-commit install
```
## License

Apache License 2.0
