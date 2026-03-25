# Ansible Lab Environment DevContainer

This project provides a standardized, containerized workspace designed specifically for ansible. It uses VS Code Dev Containers to ensure you are practicing with the exact toolset required.

## Prerequisites

### 🛠️ Required Tools
* **Visual Studio Code**
* **Docker** or **Podman**

### 🧩 VS Code Extensions
* **Dev Containers**: Allows VS Code to run inside the environment.
* **Ansible (by Red Hat)**: Provides syntax highlighting and `ansible-navigator` integration.

---

## Installation & Setup

### 1. Project Structure
Ensure your project root contains the following configuration files:

```text
.
├── .devcontainer/
|   └── docker/
|         └── devcontainer.json
|   └── podman/
|         └── devcontainer.json
│   └── devcontainer.json
├── ansible-navigator.yml
└── README.md
```

If you are using podman your devcontainer.json need to use mounts:
```
  "mounts": [
    "source=${localEnv:XDG_RUNTIME_DIR}/podman/podman.sock,target=${localEnv:XDG_RUNTIME_DIR}/podman/podman.sock,type=bind"
  ],
```
### 2. Pull the Image

Before launching, pull the community development image. This is a public image and does not require any login:

```
docker pull ghcr.io/ansible/community-ansible-dev-tools:latest
```
### 3. Launch the Environment

Reload your workspace into the container:

1) Open the Command Palette: CTRL + SHIFT + P
2) Select: "Dev Containers: Reopen in Container"

If VS Code detects the folder automatically, you can simply click the "Reopen in Container" notification in the bottom-right corner.

### 4. Verification

To ensure your environment is configured correctly, verify the tools and the Execution Environment:

1) Open a terminal in VS Code (CTRL + ~).
2) Run the following command:
```
ansible-navigator doc -l
```
You should see the full list of available modules provided by the quay.io/ansible/creator-ee:latest image.

### 5. Tips Ansible:

[Tips: Best Practice Ansible ](./Tips-ansible.md)

### 6. Simulation Environment Setup:

To simulate a real-world RHEL environment within Podman, we use a custom-built managed node that supports systemd and SSH. This allows for full service management and user configuration testing.

1) Build the Target Image:
```
podman build -t my-ansible-target -f Containerfile.target .
```

2) Establish the Network:
```
podman network create ansible-lab
```

3) Connect the DevContainer:
```
.....
  "runArgs": [
    "--network=ansible-lab",
    "--privileged" // Often needed to run ansible-navigator or nested podman
  ],
....
```

4) Launch the Managed Node:
```
podman run -d \
  --name target-server \
  --network ansible-lab \
  --privileged \
  my-ansible-target
```

5) Configure Ansible

Create inventory:
```
[webservers]
target-server ansible_user=root ansible_password=redhat 
```

Create ansible.cfg:
```
[defaults]
collections_path=/root/.ansible/collections:/usr/share/ansible/collections
inventory=./inventory
remote_user=devops
host_key_checking=false
roles_path=/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles

[privilege_escalation]
become=True
become_ask_pass=False
become_method=sudo
become_user=root

```

6) Verify Connectivity:
```
ansible webservers -m ping 
```

### 💡 Troubleshooting: Remote Host Identification
If the target container is deleted and recreated, its SSH fingerprint will change. Even with host_key_checking = false, your local shell may block the connection if the old key is saved.

Fix: Remove the stale entry from your known hosts:
```
rm /root/.ssh/known_hosts
```

#### Tools with this image :

Example install collection:
```
ansible-galaxy collection install fedora.linux_system_roles -p /home/student/ansible/collections
```

#### Configuration ansible-navigator.yml:

```
ansible-navigator settings --sample > ansible-navigator.yml
```
