# Varnish Ansible Role

[![CI](https://github.com/bilalcaliskan/varnish-ansible-role/workflows/CI/badge.svg?event=push)](https://github.com/bilalcaliskan/varnish-ansible-role/actions?query=workflow%3ACI)

Installs and configures Varnish cache server on RHEL/CentOS 7/8 instances.

## Requirements

This role requires minimum Ansible version 2.4 and maximum Ansible version 2.9. You can install suggested version with pip:
```
$ pip install "ansible==2.9.16"
```

No special requirements; note that this role requires root access, so either run it in a playbook with a global `become: true`, or invoke the role in your playbook.

## Role Variables
See the default values in [defaults/main.yml](defaults/main.yml). You can overwrite them in [vars/main.yml](vars/main.yml) if neccessary or you can set them while running playbook.

> Please note that this role will ensure that `firewalld` systemd service on your servers are started and enabled by default. If you want to stop and disable `firewalld` service, please modify below variable as false when running playbook:
> ```yaml
> firewalld_enabled: false

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

### License

Apache License 2.0
