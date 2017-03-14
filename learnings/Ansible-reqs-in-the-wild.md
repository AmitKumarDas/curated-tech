### Tutorial starts ...

```yaml
References:
  - https://www.digitalocean.com/community/tutorials/
  - https://github.com/leucos/ansible-tuto/
Set Up:
  - Ansible:
    - Get the usual downloads
    - SSH Keys:
      - Create RSA key pair via ssh-keygen
      - Keys will be at user's .ssh i.e. ~/.ssh folder
      - Add the contents of ~/.ssh/id_rsa.pub in other hosts
    - Ansible Hosts:
      - touch /etc/ansible/hosts:
        - you can group the hosts 
        - each host will have an alias & ip address
        - a host can be on multiple groups
    - Specific Ansible Host:
      - When we need to configure each host exclusivley:
        - mkdir /etc/ansible/host_vars
        - touch /etc/ansible/host_vars/some_name
    - Enable SSH using root user:
      - mkdir /etc/ansible/group_vars
      - touch /etc/ansible/group_vars/host_group_name:
          - i.e. name of the host group defined in /etc/ansible/hosts
        - Contents:
          - ---
          - ansible_ssh_user: root
Modular Approach:
  - Ansible:
    - Lots of modules available
  - Writing a new module is pretty easy:
    - It doesn't even have to be Python, it just needs to speak JSON
Config:
  - Ansible: 
    - YAML files
    - starts with ---
Playbook Approach:
  - set of tasks targeted to specific hosts
Ansible interacts with clients:
  - via client CLI
  - via its playbook i.e. configuration scripts
Ansible CLI:
  - -m option refers to module
    - e.g. -m ping
  - -a option refers to arguments
    - e.g. ansible -m shell -a 'free -m' host1
Snips:
  - ansible cli basics:
    - ansible -m ping all
    - ansible -m ping particular_host
    - ansible -m ping host_group_name
    - ansible -m ping host1:host2
  - just hosts:
    - host0.example.org ansible_host=192.168.33.10 ansible_user=root
    - host1.example.org ansible_host=192.168.33.11 ansible_user=root
    - host2.example.org ansible_host=192.168.33.12 ansible_user=root
  - grouping hosts basics:
    ```ini
    [debian]
    host[0:2].example.org
    [ubuntu]
    host0.example.org
    [linux:children]
    ubuntu
    debian
    ```
  - playbook basics:
    ```yaml
    - hosts: web
      tasks:
        - name: Installs apache web server
          apt: pkg=apache2 state=installed update_cache=true
    ```
Quick Tips:
  - Modules:
    - setup:
      - gathers node's facts e.g. IP, architecture, bios, etc
  - Folders:
    - /etc/ansible/host_vars
    - /etc/ansible/group_vars
  - Files:
    - /etc/ansible/hosts
    - ~/.ansible.cfg
  - CLI:
    - ansible-playbook
    - ansible
  - CLI Options
    - -i provides the inventory path
    - --extra-vars or -e
  - Environment Variables:
    - ANSIBLE_HOSTS
  -Special Variables:
    - ansible_host
    - ansible_port
    - ansible_user / ansible_ssh_user
```
