# Tips ansible

## Build a ansible.cfg

generate an ansible.cfg

```
ansible-config init --disable > ansible.cfg
```

Most important in the file :

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

check if it s working:

```
ansible --version
```

Result: config file = /workspaces/test-exam/ansible.cfg

If not:

```
chmod 755 /workspaces/test-exam
```
