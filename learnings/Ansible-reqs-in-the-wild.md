### Tutorial starts ...

```yaml
References:
  - https://www.digitalocean.com/community/tutorials/
Set Up:
  - Ansible:
    - Usual downloads
    - SSH Keys:
      - Create RSA key pair via ssh-keygen
      - Keys will be at user's .ssh i.e. ~/.ssh folder
      - Add the contents of ~/.ssh/id_rsa.pub in other hosts
    - Ansible Hosts:
      - touch /etc/ansible/hosts
      - you can group the hosts 
      - each host will have an alias & ip address
      - a host can be on multiple groups
    - Specific Ansible Host:
      - When we need to configure each host exclusivley
      - mkdir /etc/ansible/host_vars
      - touch /etc/ansible/host_vars/some_name
    - Enable SSH using root user
      - mkdir /etc/ansible/group_vars
      - touch /etc/ansible/group_vars/host_group_name:
        - i.e. name of the host group defined in /etc/ansible/hosts
        - Contents:
          - ---
          - ansible_ssh_user: root
  - Kubernetes:
    - Do nothing
    - You will learn it once you read this template
Modular Approach:
  - Ansible:
    - Is the way of life
    - Write module in any language
    - Communicate with standard json
Config:
  - Ansible: 
    - YAML files
    - starts with ---
Ansible interacts with clients:
  - via client CLI
  - via its playbook i.e. configuration scripts
Ansible CLI:
  - -m option refers to module
    - e.g. -m ping
  - -a option refers to arguments
    - e.g. ansible -m shell -a 'free -m' host1
Verify:
  - Ansible:
    - ansible -m ping all
    - ansible -m ping particular_host
    - ansible -m ping host_group_name
    - ansible -m ping host1:host2
Ansible Quick Tips:
  - Folders:
    - /etc/ansible/host_vars
    - /etc/ansible/group_vars
  - Files:
    - /etc/ansible/hosts
  - Options:
    - ansible_ssh_user: root
```
