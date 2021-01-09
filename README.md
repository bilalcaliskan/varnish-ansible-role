## Varnish Ansible Role

[![Build Status](https://travis-ci.org/bilalcaliskan/varnish-ansible-role.svg?branch=master)](https://travis-ci.org/bilalcaliskan/varnish-ansible-role)

Installs and configures Varnish cache on RHEL/CentOS 7/8 instances.

### Requirements

No special requirements; note that this role requires root access, so either run it in a
playbook with a global `become: true`, or invoke the role in your playbook like:

```yaml
- hosts: all
  become: true
  roles:
    - role: bilalcaliskan.varnish
      vars:
        simple_role_var: foo
```

### Role Variables

See the default values in [defaults/main.yml](defaults/main.yml). You can overwrite them in [vars/main.yml](vars/main.yml) if neccessary or you can set them while running playbook.

> Please note that this role will ensure that `firewalld` systemd service on your servers are started and enabled by default. If you want to stop and disable `firewalld` service, please modify below variable as false when running playbook:  
> ```yaml  
> firewalld_enabled: false


## Dependencies

None

### Example Inventory File

```
[varnish]
node01.example.com
node02.example.com
node03.example.com
```

### Example Playbook File For Installation
```yaml
- hosts: varnish
  become: true
  roles:
    - role: bilalcaliskan.varnish
      vars:
        varnish_install: true
```

### Example Playbook File For Uninstallation

```yaml
- hosts: varnish
  become: true
  roles:
    - role: bilalcaliskan.varnish
      vars:
        varnish_install: false
```

### License

MIT / BSD
