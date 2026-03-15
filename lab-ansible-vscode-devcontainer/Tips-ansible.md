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
host_key_checking=False

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