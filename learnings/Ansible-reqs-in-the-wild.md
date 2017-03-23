
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

```yaml
Inventory:
  - /etc/ansible/hosts:
    host0.example.org ansible_host=192.168.33.10 ansible_user=root
    host1.example.org ansible_host=192.168.33.11 ansible_user=root
    host2.example.org ansible_host=192.168.33.12 ansible_user=root
```

```yaml
Verify Till Now:
  - ansible -m ping all -i step-01/hosts
  - ansible -i step-02/hosts -m shell -a 'uname -a' host0.example.org
  - ansible -i step-02/hosts -m shell -a 'grep DISTRIB_RELEASE /etc/lsb-release' all
```

```ini
;Inventory [<groupname>:children]:
[ubuntu]
host0.example.org

[debian]
host[1:2].example.org

[linux:children]
ubuntu
debian
```
