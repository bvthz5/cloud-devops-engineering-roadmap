# 01 - Ansible Architecture & Agentless Push

> **Learning Methodology:**
> **Understand -> See -> Practice -> Troubleshoot -> Interview -> Revise**

## Module Syllabus
1. `01-Ansible-Control-Node-vs-Managed-Nodes.md` - Control node requirements, managed nodes, SSH, WinRM, and Python dependencies.
2. `02-Agentless-Architecture-and-SSH-Push-Model.md` - Why agentless is advantageous, how push architecture works vs Puppet/Chef pull models.
3. `03-Ansible-Execution-Flow-and-Module-Transport.md` - Under the hood: payload generation, SFTP/SCP transfer, execution, and cleanup.
4. `04-Ansible-Configuration-ansible-cfg-Hierarchy.md` - ANSIBLE_CONFIG environment variable, local ansible.cfg, ~/.ansible.cfg, /etc/ansible/ansible.cfg precedence.
5. `05-SSH-Key-Management-Sudo-and-Privilege-Escalation.md` - become, become_user, become_method, sudoers configuration, and passwordless sudo.
6. `06-Ansible-Engine-vs-Core-vs-Collections-Evolution.md` - History, ansible vs ansible-core, plugin architecture, and collection namespace.
7. `07-Real-World-Scenarios.md` - Bastion host setup, SSH jump hosts, and managing multi-tier Linux/Windows infrastructure.
8. `08-Troubleshooting.md` - SSH host key verification failure, permission denied publickey, sudo password errors, and missing Python.
9. `09-Interview-QA.md` - Interview questions on architecture, agentless model, and privilege escalation.
10. `10-Hands-On-Practice.md` - Hands-on lab: setup control node, configure SSH keys, test ansible ping.
11. `11-MCQ.md` - 10 multiple choice questions with explanations.
12. `12-Quick-Revision.md` - Cheat sheet and architectural summaries.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Section 09 - IaC (Terraform)](../README.md) | [README](./README.md) | [01 - Control Node vs Managed Nodes](./01-Ansible-Control-Node-vs-Managed-Nodes.md) |
