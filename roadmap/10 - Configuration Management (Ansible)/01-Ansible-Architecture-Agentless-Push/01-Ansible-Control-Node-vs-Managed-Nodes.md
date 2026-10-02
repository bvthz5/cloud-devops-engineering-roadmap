# 01 - Control Node vs Managed Nodes

## 1. Concept Overview

Ansible operates on a strict **Control Node / Managed Node** architecture. Unlike legacy configuration management tools (Puppet, Chef, SaltStack), Ansible requires **no agent daemon** installed on managed nodes.

```text
+-----------------------------------+
|       ANSIBLE CONTROL NODE        |
|  (Linux/macOS - Python 3.9+)     |
|  - Ansible Core Engine            |
|  - Playbooks & Inventories        |
|  - SSH / WinRM Client             |
+-----------------+-----------------+
                  |
        SSH (22)  |  WinRM (5986)
                  v
+-----------------+-----------------+
|          MANAGED NODES            |
|  Linux / Unix / Windows / Network |
|  - Python 3.7+ (Linux)            |
|  - PowerShell 5.1+ (Windows)      |
|  - NO PERMANENT AGENT DAEMON!     |
+-----------------------------------+
```

### Key Differences
- **Control Node**: The machine where Ansible is installed and executed. Must be Linux, Unix, or macOS (Windows control nodes are NOT natively supported, though WSL2 works).
- **Managed Nodes**: The target servers, network devices, cloud instances, or containers managed by Ansible. Python 3 is required on Linux target nodes to execute modules.

---

## 2. Technical Architecture & Requirements

| Property | Control Node | Managed Node (Linux) | Managed Node (Windows) |
|---|---|---|---|
| **Supported OS** | RHEL, Ubuntu, Debian, macOS, WSL2 | Any Linux kernel 2.6+ | Windows Server 2016+, Win 10/11 |
| **Python Requirement** | Python 3.9+ | Python 3.7+ (or python3-apt for Debian) | N/A (PowerShell 5.1+ & .NET 4.0+) |
| **Agent Installed?** | N/A | **NO** | **NO** |
| **Connection Protocol** | Local CLI (`ansible-playbook`) | SSH (OpenSSH) | WinRM or SSH (OpenSSH for Windows) |
| **Port Required** | Outbound SSH/WinRM ports | Inbound Port 22 (SSH) | Inbound Port 5986 (HTTPS WinRM) |

---

## 3. Real-World Architecture Example

```ini
# /etc/ansible/ansible.cfg snippet
[defaults]
inventory      = ./hosts
remote_user    = devops
private_key_file = ~/.ssh/ansible_ed25519
host_key_checking = False
timeout        = 30
```

---

## 4. Key Takeaways
- Control nodes run Ansible CLI and playbooks.
- Managed nodes only require an SSH server and Python 3 interpreter.
- No central server agent or background daemon consumes memory on target systems.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Section (09 - Infrastructure as Code (Terraform & OpenTofu))](../../09%20-%20Infrastructure%20as%20Code%20(Terraform%20%26%20OpenTofu)/16-CDK-for-Terraform-and-Pulumi-Comparison/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Agentless Architecture and SSH Push Model →](./02-Agentless-Architecture-and-SSH-Push-Model.md) |
