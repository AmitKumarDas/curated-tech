
### github.com/leucos/ansible-tuto/

```yaml
Quick Tutorial:
  - https://github.com/leucos/ansible-tuto/
```

```yaml
Installs:
  $ ansible --version
    ansible 2.0.0.2
      config file = /etc/ansible/ansible.cfg
      configured module search path = Default w/o overrides  
```

```yaml
Setup:
  - Create VMs via Vagrant
  - Add your laptop's ssh keys on above VMs:
    - ansible-playbook -c paramiko -i step-00/hosts step-00/setup.yml --ask-pass --become  
```

