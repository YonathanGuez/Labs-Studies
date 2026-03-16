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
│   └── devcontainer.json
├── ansible-navigator.yml
└── README.md
```

If you are using podman you need to modify devcontainer.json in the part mounts:
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