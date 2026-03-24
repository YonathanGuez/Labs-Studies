# Tips ansible

## Build a ansible.cfg

### generate an ansible.cfg

```
ansible-config init --disable > ansible.cfg
```

### Most important in the file:
default : collections_path, inventory, remote_user, roles_path
privilege_escalation: become, become_ask_pass, become_method, become_user

Example:
```
[defaults]
collections_path=./my-collection:/usr/share/ansible/collections
inventory=./inventory
remote_user=devops
roles_path=./my-roles:/usr/share/ansible/roles:/etc/ansible/roles

[privilege_escalation]
become=True
become_ask_pass=False
become_method=sudo
become_user=root
```

### check the connection working:

```
ansible --version
```

Result: config file = /workspaces/test-exam/ansible.cfg

If not:

```
chmod 755 /workspaces/test-exam
```

## Inventory

Example of inventory file:
```
[lab]
servera.lab.example.com
[preprod]
serverb.lab.example.com
[production]
serverc.lab.example.com
serverd.lab.example.com
[myprod]
serverd.lab.example.com
[webserver:children]
production
myprod
```

Build an all tree of inventory: 
```
ansible-inventory --graph
```

Test the connection :
```
ansible all -m ping
```

## test install Role :

create requierments.yml
```
- src: https://galaxy.ansible.com/download/openafs_contrib-openafs-1.9.0.tar.gz
  name: openafs-devops-wala
```
install
```
ansible-galaxy role install -r requirements.yml -p /home/devops/ansible/roles
```

Check:
```
ansible-galaxy role list -p /home/devops/ansible/roles
```

## System Role configuration NTF
Objectif: 
Install the rhel-system-roles package & create a playbook /home/devops/ansible/timesync.yaml
 Runs on ALL managed nodes
 Uses the timesync role 
 Configures the role to use the currently active NTF provider (more on this later)
 Configure the role to use the time server 172.256.254.254 or classroom.example.com
 Configure the role to enable the iburst parameter

rhel-system-roles package not possible with my fedora so this is the same (this is the install with the doc)
```
dnf install -y linux-system-roles --setopt=tsflags=
```

check example in :cat /usr/share/doc/linux-system-roles/timesync/README.md
```
- name: Manage timesync with 3 servers
  hosts: targets
  vars:
    timesync_ntp_servers:
      - hostname: foo.example.com
        iburst: true
      - hostname: bar.example.com
        iburst: true
  roles:
    - linux-system-roles.timesync
```

ansible-playbook  timesync.yaml --syntax-check
check : chonyc sources -v